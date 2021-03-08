---
title: Java LinkedList
author: junga park
date: 2020-03-26 12:00:00 +0800
categories: [., java]
tags: [java, LinkedList]
---


# LinkedList

Queue와 List 인터페이스를 구현한 클래스



### 사용방법

```java
Queue<E> queue = new LinkedList<E>();
```



### 메소드

##### boolean add(E e) / boolean add(int index, E e)

객체를 맨뒤에 추가. 인덱스를 통해 추가하고 싶은 위치도 지정할 수 있다.



##### void clear()

리스트에 존재하는 모든 객체를 제거



##### boolean contains(Object o)

해당 객체가 존재하면 true 반환



##### E peek()

첫번째 객체(가장 먼저 넣은 객체)를 반환한다. 제거하지 않는다.



##### E poll()

첫번째 객체(가장 먼저 넣은 객체)를 제거하고 반환한다.



##### boolean remove(int index) / boolean remove(Object o)

리스트에서 해당 객체를 제거한다. 인덱스를 지정할 수도 있고, 객체를 지정할 수도 있다.



##### int size() 

리스트에 존재하는 객체의 개수를 반환한다.



### 예시

```java
import java.util.LinkedList;

public class QueueExample2 {

    public static void main(String[] args) {
        LinkedList<Integer> integerList = new LinkedList<>();

        // 데이터 추가
        integerList.offer(1);
        integerList.offer(2);
        integerList.add(3);
        integerList.add(4);

        // 데이터가 존재하지 않을때까지 삭제, 출력
        while (!integerList.isEmpty()) {
            System.out.println(integerList.poll());
        }
    }
}
```

**출력**

```
1
2
3
4
```





### remove 예시

리스트에서 숫자 "3"을 제거하려면 어떻게 할까?

```java
integerList.remove(3);
```

다음을 사용하여 제거 할 경우에는 3 번째 인덱스로 인식되기 때문에 출력값은 다음과 같아진다.

**출력**

```
1
2
3
```



숫자를 객체로 인식하게 하기 위해서는 다음과 같이 해준다.

```java
integerList.remove(Integer.valueOf(3));
```

**출력**

```
1
2
4
```

Integer.valueOf()는 정수형 인스턴스로 반환해주는 메소드이다.

---

### add와 offer의 차이

offer()와 add()는 둘 다 데이터를 리스트에 추가하는 동일한 기능을한다. 그렇지만 차이점은 실패 시, add는 예외를 발생하고, offer()은 false를 반환한다.

동일한 예로 다음과 같은게 있다.

##### 예외발생

- add()
- remove()
- element()

##### false 반환

- offer()
- poll()
- peek()



---

### linkedList Vs arrayList

- 객체를 추가하거나 삭제할 때 arrayList보다 빠르다.
- 요소에 대한 접근속도가 느리다.