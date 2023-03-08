---
title: "[Python] Class"
last_modified_at: 2020-12-13T02:58
tags: 
  - 'class' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[점프 투 파이썬](https://wikidocs.net/28)

[파이썬 - 기본을 갈고 닦자!](https://wikidocs.net/16075)

[]()

[]()

[]()

[]()

<br>

# Class


## self

**Python method의 첫 번째 매개변수 이름**은 **관례적으로 `self`를 사용**한다.
_객체를 호출할 때 호출한 객체 자신이 전달되기 때문에 `self`를 사용한 것_
_`self`말고 다른 이름을 사용해도 상관없다._

**method의 첫 번째 매개변수 `self`를 명시적으로 구현**하는 것은 **Python만의 특징**이다.


## `__init__`

**Python에서 method 이름**으로 **`__init__`을 사용하면 생성자(Constructor)**가 된다.

## pass

**`pass`는 아무것도 수행하지 않는 문법**으로 **임시로 코드를 작성할 때 주로 사용**한다.

## import

다른 언어에서 처럼 `import`로 만들어진 `.py`에 Class를 생성하여 아래 처럼 사용하려고 하면 에러가 발생 한다.

```python
# Calculator.py

class Calculator:
    def __init__(self, firstNumber, secondNumber):
        self.firstNumber = firstNumber
        self.secondNumber = secondNumber

    def setNumber(self, firstNumber, secondNumber):
        self.firstNumber = firstNumber
        self.secondNumber = secondNumber

    def add(self):
        resultNumber = self.firstNumber + self.secondNumber
        return resultNumber

    def multiple(self):
        resultNumber = self.firstNumber * self.secondNumber
        return resultNumber

    def sub(self):
        resultNumber = self.firstNumber - self.secondNumber
        return resultNumber

    def divide(self):
        resultNumber = int(self.firstNumber / self.secondNumber)
        return resultNumber


# CalculatorTest.py

import Calculator

calculatorA = Calculator(1, 2)

print(calculatorA.add())

'''
실행 결과 : TypeError: 'module' object is not callable
'''

```

`import`한 `Module`을 Class 처럼 사용하려해서 발생한 에러로, `import`한 `Module`은 Class처럼 사용할 수 없다.

보통 FileName과 ClassName을 같이 매칭하여 사용할 때 많이 발생한다.

아래와 같이 `import파일명.class명()`하여 사용해야 한다.

```python
# CalculatorTest.py

import Calculator

calculatorA = Calculator.Calculator(1, 2)

print(calculatorA.add())

# 실행결과 : 3
```

## 상속


```python
# Calculator.py

class Calculator:
    def __init__(self, firstNumber, secondNumber):
        self.firstNumber = firstNumber
        self.secondNumber = secondNumber

    def setNumber(self, firstNumber, secondNumber):
        self.firstNumber = firstNumber
        self.secondNumber = secondNumber

    def add(self):
        resultNumber = self.firstNumber + self.secondNumber
        return resultNumber

    def multiple(self):
        resultNumber = self.firstNumber * self.secondNumber
        return resultNumber

    def sub(self):
        resultNumber = self.firstNumber - self.secondNumber
        return resultNumber

    def divide(self):
        resultNumber = int(self.firstNumber / self.secondNumber)
        return resultNumber
        
class ChildCalculator(Calculator):
    def add(self):
        print("Overriding")
        return self.firstNumber, self.secondNumber

# CalculatorTest.py

import Calculator

calculatorA = Calculator.Calculator(1, 2)

calculatorB = Calculator.Calculator(1, 22)

calculatorC = Calculator.ChildCalculator(1, 22)



print(calculatorA.add())
print(calculatorB.add())

print(calculatorC.add())

'''
3
23
Overriding
(1, 22)
'''

```

### 다중 상속

**`Python`은 `C#`, `Java`와 다르게 `C++`과 같이 다중 상속을 지원**한다.

```python
# MultiInheritance.py

class Human:
    def talk(self):
        print("말을 한다.")
    def eat(self):
        print("overriding없이 먹는다.")

class Developer:
    def coding(self):
        print("coding 한다.")

class Gillog(Human, Developer):
    def talk(self):
        print("gillog는 overriding 하여 말을 한다")
    def coding(self):
        print("gillog는 overriding 하여 코딩 한다")

# MultiInheritanceTest.py

import MultiInheritance

human = MultiInheritance.Human()

developer = MultiInheritance.Developer()

gillog = MultiInheritance.Gillog()

human.talk()

developer.coding()

gillog.eat()
gillog.talk()
gillog.coding()

'''
말을 한다.
coding 한다.
overriding없이 먹는다.
gillog는 overriding 하여 말을 한다
gillog는 overriding 하여 코딩 한다
'''
```

### super()

`super()`는 자식 Class 내에서도 부모 Class를 호출 할 수 있다.

### mro()

`Class.mro()`는 해당 Class의 상속 관계를 알 수 있는 method 이다.

## @abstractmethod

`Class`에 method 위에 `@abstractmethod`을 추가하면 추상 메소드를 추가하여 해당 Class를 추상 Class로 만들 수 있다.

반드시 `abc` module을 `import`해야 한다.

`from abc import *`

```python
from abc import *

class AbstractGil(metaclass=ABCMeta):
    name = '이름'

    @abstractmethod
    def showName(self):
        print("이름은?")

class Gillog(AbstractGil):

    def __init__(self, name):
        self.name = name

gillog = Gillog("gillog")

'''
TypeError: Can't instantiate abstract class Gillog with abstract methods showName
'''
```


추상 method `showName()` 을 구현하지 않았기에 error가 발생한다.

```python
from abc import *

class AbstractGil(metaclass=ABCMeta):
    name = '이름'

    @abstractmethod
    def showName(self):
        print("이름은?")

class Gillog(AbstractGil):

    def __init__(self, name):
        self.name = name
    def showName(self):
        super().showName()
        print(self.name, "입니다.")

gillog = Gillog("gillog")

gillog.showName()

'''
이름은?
gillog 입니다.
'''
```

### @classmethod, @staticmethod

정적 method는 Class에서 직접 접근할 수 있는 method이다.

python에서 Class에서 접근할 수 있는 method는 `staticmethod`와 `classmethod` 두 가지가 있다.

**Python에서는 다른 언어들과 다르게 Instance에서도 static method 접근이 가능하다.**


**둘의 차이점은 `staticmethod`에서는 부모 Class의 Class 속성 값**을 가져오지만, **`classmethod`에서는 self의 Class 속성 값**을 가져온다.

```python
class Language:
    default_language = "한국어"

    def __init__(self):
        self.show = '나의 언어는 ' + self.default_language

    @classmethod
    def classMyLanguage(self):
        return self()

    @staticmethod
    def staticMyLanguage():
        return Language()

    def printLanguage(self):
        print(self.show)


class secondLanguage(Language):
    default_language = "영어"


staticmethod = secondLanguage.staticMyLanguage()
classmethod = secondLanguage.classMyLanguage()

staticmethod.printLanguage()
classmethod.printLanguage()

'''
나의 언어는 한국어
나의 언어는 영어
'''
```