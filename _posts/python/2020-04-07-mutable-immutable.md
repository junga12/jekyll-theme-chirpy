---
title: mutable, immutable 변수
author: Junga Park
date: 2020-04-07 12:00:00 +0800
categories: [., python]
tags: [python, mutable, immutable, deep copy, shallow copy]
---

파이썬의 변수는 mutable한 변수와 immutable한 변수로 나눌 수 있다.

# mutable

값이 변하는 변수

예를 들어, List, Dictionary가 있다.

# immutable 

값이 변하지 않는 변수

예를 들어, Number, String, Tuple가 있다.



### mutable한 변수를 어떻게 복사할까

##### 1  shallow copy

```python
x = [1, 2, 3]
y = x
x.append(4)

print(x) # [1, 2, 3, 4]
print(y) # [1, 2, 3, 4]
```

```y = x```처럼 x를 복사하면 얕은 복사(shallow copy)를 하게 된다.



###### 2 슬라이싱

```python
x = [1, 2, 3]
y = x[:]
x.append(4)

print(x) # [1, 2, 3, 4]
print(y) # [1, 2, 3]
```

```y = x[:]```처럼 복사하면 깊은 복사(deep copy)를 하게 된다.

리스트 안에 리스트가 있는 경우에는 내부 리스트까지 깊은 복사를 하지 않기 때문에 아래의 방법을 사용해야 한다.



##### 3 copy 이용하기

```python
import copy
x = [1, 2, 3]
y = copy.deepcopy(x)
x.append(4)

print(x) # [1, 2, 3, 4]
print(y) # [1, 2, 3]
```

copy.deepcopy()를 사용하여 깊은 복사(deep copy)를 할 수 있다.



