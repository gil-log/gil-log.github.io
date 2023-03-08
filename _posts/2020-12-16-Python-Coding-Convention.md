---
title: "[Python] Coding Convention"
last_modified_at: 2020-12-16T06:57
categories: 
  - python
tags: 
  - 'coding convention' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[파이썬 명명규칙[히처리]](https://ruriro.tistory.com/11)

[파이썬! 지켜야할 기초 코딩 룰, 컨벤션[nophotoplease]](https://nophotoplease.tistory.com/95)

[]()

[]()

[]()

[]()

# Coding Convention ?
**`Coding Convention`은 프로그램 코드를 작성할 때 사용되는 일종의 기준**이다. 

---

# 명명규칙

## Example

```
ClassName
ExceptionName

module_name
package_name
method_name
function_name
function_parameter_name
global_var_name
local_var_name
instance_var_name

GLOBAL_CONSTANT_NAME
```


## package & module
짧은 소문자 + 언더스코어


## Class
CapWords규칙을 따른다.
_각 단어의 첫 글자를 대문자로, Car, Gil 등_


## Exception

예외는 클래스이므로 CapWords규칙을 따르고 Error 접미사를 사용한다.

## 전역 변수
언더스코어를 붙여서 해당 모듈에서만 쓰이도록 한다.

## function
소문자 + 언더스코어

## method 이름과 Instance 변수
소문자 + 언더스코어

## 상수
대문자 + 언더스코어

---

# 언더스코어의 특별한 의미

## 접미사 하나를 사용한 경우
내부에서 사용한다는 의미

## 접미사 두개를 사용한 경우
클래스 내부에 protect로 사용한다는 의미

## 접두사 하나를 사용한 경우
파이썬 키워드와의 충돌을 방지한다는 의미


---

# ETC

## tab보다는 4 space
PEP8에 따르면 공백을 표준으로 코딩해 나가길 권장하고 있다.

## 라인 당 79자 제한

```python
def function_example(
    one,
    two,
    three):
    ...
```

여기서 주의할 점은 **인자를 한 줄에 모두 쓰던가, 혹은 위와 같이 내려야 된다고 권장**한다.

```python
# 권장하지 않는 케이스
def function_example(one,
    two,
    three):
    ...
```


**다중 라인이 필요한 경우, 필요에 따라서 괄호를 `'(, )'`를 활용하는 것도 추천**된다

```python
from bul.ra.bul.ral.module import (
    so, many, libraries, are, needed)
```

## 변수명으로 'l', 'O', 'I' 피할 것
변수 이름으로 가독성이 떨어지는 위 글자 사용을 지양하도록 한다.


## import 

### import 한 줄당 하나의 library

한 줄당 하나의 라이브러리를 import 하는 것이 원칙이다.

예를 들어 아래는 권장되지 않는다

```python
import os, sys, argparse
```

### import 순서
다른 언어들과 비슷하게 아래의 순서대로 import 한다.

```
standard library
third party
local application 및 library
```

