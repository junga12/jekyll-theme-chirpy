---
title: Scanner과 BufferedReader 비교
author: Junga Park
date: 2020-03-17 12:00:00 +0800
categories: [., java]
tags: [java, Scanner, BufferedReader]
---

#### 테스트 문제

백준 2290번 LCD Test

https://www.acmicpc.net/problem/2290

---

### Scanner

Scanner을 사용하여 nextInt()와 next()를 이용하여 값을 받은 경우

```java
import java.util.Scanner;

public class Bj2290 {
    private static int width;

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int s = sc.nextInt(); // 1 <= s <= 10
        String n = sc.next(); // 0 <= n <= 9,999,999,999
        width = s + 2; // 가로 = s + 2
        
        {
            ... // 뒤의 내용 생략
        }
    }
}
```

메모리와 시간은 다음과 같이 나온다.

![20200318_01](/assets/img/java/20200318_01.JPG)

메모리는 15076 KB, 시간은 132 ms를 사용한다.



---



### BufferedReader, StringTokenizer

BufferedReader과 StringTokenizer을 사용하여 입력을 받는 경우

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Bj2290 {
    private static int width;

    public static void main(String[] args)throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int s = Integer.parseInt(st.nextToken());
        String n = st.nextToken();
        width = s + 2; // 가로 = s + 2
        
        {
            ... // 뒤의 내용 생략
        }
    }
}

```

메모리와 시간은 다음과 같이 나온다.

![20200318_02](/assets/img/java/20200318_02.JPG)

시간은  13792 KB, 시간은 104 ms가 나온다.



---



#### 결론

Scanner보다 BufferedReader가 더 빠르게 동작한다는 것을 알 수 있습니다. 속도의 차이가 나는 이유는 동작하는 방식이 다르기 때문입니다.



BufferedReader은 일정한 크기의 데이터를 한 번에 읽어와서 버퍼에 보관합니다. 이후에 사용자가 요청할 때 버퍼에서 데이터를 읽어오는 방식으로 동작하기 때문에 속도를 향상시킬 수 있습니다. BufferedReader의 realLine() 메소드는 String만 반환하기 때문에 필요한 타입으로 변형을 시켜야 한다는 번거로움이 존재합니다.



Scanner는 버퍼를 사용하지 않고, 사용자의 요청이 있을 때마다 데이터를 읽어오는 방식으로 동작합니다. 또한, 정규식을 사용하여 파싱을 하기 때문에 속도가 BufferedReader보다 느리다는 단점이 있습니다. 



| Scanner                               | BufferReader               |
| ------------------------------------- | -------------------------- |
| 빈 칸(스페이스바, 엔터)을 경계로 인식 | 줄바뀜(엔터)만 경계로 인식 |
| 데이터 가공이 쉽다                    | 데이터 가공이 필요하다     |
| 상대적으로 느리다                     | 상대적으로 빠르다          |



---



[BufferedReader 참조](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)

[ Scanner 참조](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)







