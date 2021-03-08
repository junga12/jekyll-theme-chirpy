---
title: Java BufferedReader와 StringTokenizer
author: Junga Park
date: 2020-03-20 12:00:00 +0800
categories: [., java]
tags: [java, Scanner, BufferedReader, StringTokenizer]
---

### BufferedReader

```java
br = new BufferedReader(new InputStreamReader(System.in));
String s = br.readLine();
```

BufferdReader는 입력된 데이터를 미리 버퍼에 놓아서 효율적이며, 문자열에 최적화되어 있다. 

입력을 받을 때는 ```br.readLine()```를 사용한다. readLine은 String형을 반환하며, 예외처리를 꼭 해야합니다.




### StringTokenizer

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringTokenizer st = new StringTokenizer(br.readLine(), " ");
String basicVariable = st.nextToken();

while (st.hasMoreTokens()) {
	v = st.nextToken();
}
```

StringTokenizer은 원하는 토큰으로 끊어서 문자열 을 읽을 수 있으며, BufferdReader보다 더 빠르게 입력을 받을 수 있다.

```new StringTokenizer(br.readLine(), " ");```의 두 번째 인자에 끊어 읽고싶은 기준을 넣어준다. ```" "```는 스페이스바를 기준으로 끊는다는 의미이다. ```st.hasMoreTokens()``` 는 다음 입력이 있는지 확인하며, ```st.nextToken()```을 통해서 다음 토큰을 받을 수 있다. 



---



[BufferedReader 참조](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html) 

[ StringTokenizer 참조 ](https://docs.oracle.com/javase/7/docs/api/java/util/StringTokenizer.html)





