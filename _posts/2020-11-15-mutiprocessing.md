---
title: crawling web page
author: Junga Park
date: 2020-11-15 12:00:00 +0800
categories: [., project]
tags: [python, crawling, multiprocessing, multithreading]
---

웹페이지를 크롤링하는데, 시간이 너무 오래 걸려서 크롤링하는 시간을 줄이는 방법에 대해서 알아봤습니다. 

## 공통

**라이브러리**: BeautifulSoup, requests, openpyxl

**크롤링하려는 웹 페이지**: 네이버 금융 페이지(https://finance.naver.com/)

네이버 금융 웹 페이지는 기업마다 고유한 코드를 가지고 있고, `https://finance.naver.com/item/main.nhn?code=(기업코드)`의 형식으로 기업의 데이터에 접근할 수 있습니다. 기업 코드는 하나의 엑셀 파일로 저장해놨습니다. 이를 읽고, 필요한 데이터를 가져와 동일한 엑셀 파일에 저장합니다. 엑셀 파일은 3개의 워크시트로 구성되어 있고, 각 시트에 기업 코드가 나눠서 저장되어 있으며, 각 시트의 형태는 동일합니다. 

**크롤링 하려는 웹 페이지 개수**: 총 1060개

  

## 첫번째

```python
if __name__ == '__main__':
    start_time = time.time()
    for worksheet in worksheets:
        sheet = file[worksheet]
        stock_list = get_stock_id(sheet)
        stock_length = len(stock_list)

        for i in range(stock_length):
            company_infos = get_company_information(stock_list[i][0], income=True)
            for j in range(len(company_infos)):  # 갱신
                if stock_list[i][j+1] != company_infos[j]:
                    sheet.cell(row=i+3, column=6+j).value = company_infos[j]

    file.save("")  # 파일 저장
    print(time.time()-start_time)
```

하나의 프로세스, 하나의 스레드에서 순차적으로 진행했습니다.

약 220초 정도의 시간이 소요되었습니다.





## 두번째

##### 멀티 프로세싱

```python
from multiprocessing import Pool

def multi_processing(worksheet):
    ### do something

if __name__ == '__main__':
    start_time = time.time()
    with Pool(processes=3) as pool:
        sheet_list = pool.map(multi_processing, worksheets)

    file.save("")  # 파일 저장
    print(time.time()-start_time)

```

multi_processing 함수는 수행 되지만, 함수 내에서 수행된 데이터의 값이 file.save()를 통해서 저장되지 않습니다. 

multi_processing 함수 내에서 ```print(id(file))```을 수행하게 되면 전부 다른 값이 출력됩니다. 물론, main함수의 file값과 다른 주소값을 가지고 있습니다. 이유는 새로운 프로세스를 생성할 때 사용하는 자원도 새로 생성하기 때문에 각 프로세스가 접근하는 데이터가 다르기 때문입니다. 수행하는 데이터 주소값이 다르기 때문에 당연하게도 file.save()를 할 때, multi_processing 함수에서 수행된 내용은 저장되지 않습니다.

그래서 각 수행결과를 기존 파일로 옮깁니다.

```python
        for sheet in sheet_list:
            file.copy_worksheet(sheet)
```

 `ValueError: Cannot copy between worksheets from different workbooks`과 같은 오류가 발생했습니다. `openpyxl`의 `copy_worksheet`는 동일한 워크북에서만 이용할 수 있기 때문에 발생했습니다. 즉, 위의 코드에서 `file`과 `sheet`는 다른 워크북이기 때문에 오류가 발생했습니다.

그래서 다음과 같이 하나의 셀씩 읽고 쓰는 과정을 반복했습니다.

```python
for sheet in sheet_list:
    ws = file[sheet.title]
    mr = sheet.max_row
    for i in range(3, mr + 1):
        for j in range(6, 13):
            c = sheet.cell(row=i, column=j).value
            if c is None: continue
            ws.cell(row=i, column=j).value = c
```

3개의 프로세스를 사용하여 약 87초의 시간이 소요되었습니다.



## 세번째

##### 멀티스레딩

병렬처리를 하는 방법으로는 멀티 프로세스와 멀티 스레드 방식이 존재합니다. 멀티 프로세스 방식은 두 번째 방법으로 했기 때문에 멀티 스레드 방식도 수행해봤습니다. 멀티 스레드 방식은 총 두가지 방법을 시도해봤습니다.



먼저, 두번째의 멀티 프로세스 방식과 동일하게 하나의 워크시트를 하나의 스레드에 대입하는 방식입니다.

```python
def multi_threading(worksheet):
    sheet = file[worksheet]
    stock_list = get_stock_id(sheet)
    stock_length = len(stock_list)
    for i in range(stock_length):
        company_infos = get_company_information(stock_list[i][0], income=True)
        for j in range(len(company_infos)):  # 갱신
            if stock_list[i][j + 1] != company_infos[j]:
                sheet.cell(row=i + 3, column=6 + j).value = company_infos[j]

if __name__ == '__main__':
    thread_list = []
    with ThreadPoolExecutor(max_workers=3) as executor:
        for worksheet in worksheets:
            thread_list.append(executor.submit(multi_threading, worksheet))
        for executor in concurrent.futures.as_completed(thread_list):
            executor.result()
    file.save("")  # 파일 저장
```

이 방법은 약 110초의 시간이 소요되었습니다. 방법은 두번째 방식과 동일했지만 약 20초 정도 느린 결과를 보여줬습니다.



## 네번째

##### 멀티스레딩

네번째 방법은 앞의 두개(두번째와 세번째)의 방식과 다르게 워크시트를 신경쓰지 않고, n개의 스레드를 이용하여 순서대로 처리하는 방법입니다.

```python
def multi_threading(stock_list, loc):
    company_infos = get_company_information(stock_list[loc][0], income=True)
    for j in range(len(company_infos)):  # 갱신
        if stock_list[loc][j + 1] != company_infos[j]:
            sheet.cell(row=loc + 3, column=6 + j).value = company_infos[j]
            
if __name__ == '__main__':
    for worksheet in worksheets:
        sheet = file[worksheet]
        stock_list = get_stock_id(sheet)
        stock_length = len(stock_list)
        thread_list = []
        func = partial(multi_threading, stock_list)
        with ThreadPoolExecutor(max_workers=5) as executor:
            for i in range(len(stock_list)):
                thread_list.append(executor.submit(func, i))
            for executor in concurrent.futures.as_completed(thread_list):
                executor.result()
    file.save("")  # 파일 저장
```

해당 부분의 `max_workers`를 조절하여 최대 스레드의 수를 조절할 수 있습니다.

```python
with ThreadPoolExecutor(max_workers=3) as executor:
```

앞의 방식과 동일하게 스레드의 수를 3개로 하면 약 96초가 걸렸습니다. 두번째 방법보다는 느리지만 세번째 방법보다 빠른 속도를 보여줍니다. 

세번째 방법과 네번째 방법은 둘 다 멀티 스레드를 사용하였고, 사용한 스레드의 개수도 3개로 동일하지만 약 14초의 속도 차이가 났습니다. 이유는 각 워크시트마다 존재하는 행의 수가 다르기 때문이라고 생각합니다. 첫번째 워크시트에는 약 200개, 두번째와 세번째 워크시트에는 약 400개의 행이 존재합니다. 그렇기 때문에 세번째 방법의 멀티스레딩 방식에서는 세 개의 스레드가 균일하게 작업을 배정받지 못합니다. 반면에, 네번째 방법의 멀티스레딩 방식은 세개의 스레드가 거의 균일하게 작업을 수행하기 때문에 더 빠른 속도를 낸다고 생각합니다. 

이 방식의 멀티스레딩은 사용하는 스레드의 개수에 따라 약간의 속도 차이를 보여줄 수있습니다. `max_workes`의 수를 조절하면서 테스트해 본 결과, 사용하는 스레드의 수가 5개인 경우가 제일 좋은 결과를 보여주는 것 같습니다. 3개일 때는 약 96초, 4개일 때는 약 95초, 5개일 때는 약 94초, 6개일 때는 약 96초로, 5개가 넘어가는 순간부터는 소요되는 시간의 차이가 별로 없거나 반대로 증가하는 결과를 보여줬습니다.



---

[openpyxl](https://openpyxl.readthedocs.io/en/stable/tutorial.html)

[파이썬 multiprocessing](https://docs.python.org/ko/3/library/multiprocessing.html#pipes-and-queues)

