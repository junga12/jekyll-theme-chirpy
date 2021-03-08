---
title: Java naming conventions
author: junga park
date: 2020-03-27 12:00:00 +0800
categories: [., java]
tags: [java, namingConventions]
---

# Naming Conventions

필수는 아니지만 가독성을 높이기 위해서 사람들끼리 관습적으로 따르는 조건입니다.





### 클래스, 인스턴스 이름

- 명사로 만든다.
- 첫 번째 문자를 대문자로 표기
- 여러 단어인 경우에는 모든 단어의 첫 번째 문자만 대문자로 표기
- 단어 전체를 풀어서 사용(약어를 사용하지 않는다)
- interface Message
- class PhoneMessage



### 메소드 이름

- 동사로 만든다.
- 첫 번 째 문자를 소문자로 표기
- 여러 단어인 경우에는 카멜 표기법을 사용
- void sendTo(String to);
- void showMessage(int index)



### 변수 이름

- '_'나 '$'로 시작하지 않는다.
- 한 글자의 변수명은 사용하지 않는다.(temporary variables는 예외)
- temporary variables는 보통 i,j,k나 n,c을 사용한다.



### 상수 이름

- 모든 문자를 대문자로 표기
- 여러 단어로 만들때는 '_'를 사용
- public static final float PI = 3.1415;



### 패키지 이름

- unique하게 만든다.
- 모든 문자를 소문자로 표기



---



# 카멜 표기법 Camel Case

여러 단어로 이름을 만드는 경우에 첫 단어의 첫글자는 소문자, 나머지 단어의 첫 글자는 대문자로 표기하는 방법 (예: thisIsCamelCase)



# 파스칼 표기법 Pascal Case

모든 단어의 첫 글자를 대문자로 표기하는 방법 (예: ThisIsPascalCase)