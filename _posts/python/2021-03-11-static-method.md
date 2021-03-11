---
title: python static method
author: Junga Park
date: 2021-03-11 12:00:00 +0800
categories: [., python]
tags: [python, staticmehtod, classmethod]
---



파이썬을 사용하다가 정적 메서드를 사용해봤는데, 자세하게 정적 메서드가 무엇인지 몰라서 공부해봤다.

# 정적 메서드

일반적으로 클래스를 사용하기 위해서 인스턴스를 생성하여 사용합니다. 하지만 정적메서드는 인스턴스를 생성하지 않고 클래스를 통해서 직접 호출할 수 있습니다. 



## @staticmehtod

### 예제

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b
```

위의 예제는 두 값을 받아서 합을 반환하는 예제입니다. 정적 메서드를 사용할 때는 다음과 같이 사용합니다.

### 사용방법

```python
Calculator.add(1, 2)  # 인스턴스를 사용하지 않고, 클래스에서 바로 호출
```

물론, 인스턴스를 사용하여 다음과 같이 사용해도 아무런 문제가 없습니다.

```python
cal = Calculator()
print(cal.add(1, 2))  # 3
```



파이썬에서는 클래스 메서드를 ```def sum(self, a, b)```와 같이 ```self```를 사용합니다. 하지만 정적 메서드에서는 ```self```를 사용하지 않습니다. 즉, self를 사용하는 경우에는 에러가 발생합니다.

```python
### 에러 발생
class Calculator:
    def __init__(self):
    	self.base = 10
        
    @staticmethod
    def add(a, b):
        return a + b + self.base #### @staticmethod는 self를 사용할 수 없다.
    
```



## @classmethod

### 예시

```python
class Calculator:
    @classmethod
    def mul(cls, a, b):
        return a * b
```

@classmethod도 @staticmethod과 같은 기능을 한다. 하지만, 보는 것과 같이 @ classmethod에는 첫 번째 인자로 cls가 추가됩니다. cls는 해당 클래스를 의미합니다. 



## 차이

@staticmethod와 @classmethod는 상속할 때 차이가 납니다.

### 예시

밑을 입력으로 받고, 지수는 n으로 선언되어 있는 클래스를 생성했습니다.

```python
# N 제곱 클래스
class PowerOfN:
    n = 2  # default

    def __init__(self, base):
        self.base = base

    @staticmethod
    def static_method():
        return PowerOfN(3)

    @classmethod
    def class_method(cls):
        return cls(3)

    def getResult(self):
        return pow(self.base, self.n)


class PowerOf3(PowerOfN):
    n = 3


if __name__ == '__main__':
    n2 = PowerOfN(5)
    n2_static = n2.static_method()
    n2_class = n2.class_method()
    print(n2.getResult())  # 25 (5^2)
    print(n2_static.getResult())  # 9 (3^2)
    print(n2_class.getResult())  # 9 (3^2)

    n3 = PowerOf3(5)
    n3_static = n3.static_method()
    n3_class = n3.class_method()
    print(n3.getResult())  # 125 (5^3)
    print(n3_static.getResult())  # 9 (3^2)
    print(n3_class.getResult())  # 27 (3^3)

```

결과를 보면 알 수 있듯이 상속을 받았을 때 차이를 알 수 있습니다.

@staticmethod로 선언을 하면 상속을 받더라도 부모 클래스의 값을 사용하지만, @classmethod로 선언을하면 자식 클래스에서 선언된 값을 사용합니다.