---
title: "[Python] slicing 기본 예제"
last_modified_at: 2020-12-19T02:31
categories: 
  - python
tags: 
  - 'python' 
  - 'slicing'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Python] 파이썬 슬라이싱(slicing) 기본과 예제[TWpower's Tech Blog]](https://twpower.github.io/119-python-list-slicing-examples)

[]()

[]()

[]()

[]()

[]()

<br>


# slicing

**`slicing`은 연속적인 객체(리스트, 튜플, 문자열)의 범위를 지정해 선택한 후 객체를 가져오는 방법과 표기법을 의미**한다.

**`slicing`을 하면 새로운 객체가 생성** 된다.
_**연속적인 객체의 일부분을 복사해 가져오는 방식**_

## 기본 형태

연속적인 객체들의 자료구조(리스트, 튜플, 문자열)인 `obj`가 있다고 가정할 때, 기본 형태는 아래와 같다.

```python
obj[start : end : step]
```

`start` : **`slicing`을 시작할 시작위치**
`end` : **`slicing`을 종료할 종료 위치**로 **`end`는 포함되지 않는다.**
`step` : **`stride`라고도 하며, 옵션으로 몇개 단위로 끊어서 복사할지 정한다.**

`start`, `end`, `step`은 모두 양수, 음수를 가질 수 있다.

## index 위치

`index`가 양수 : 연속적인 객체 제일 앞에서 부터 0을 시작으로 증가한다.

`index`가 음수 : 연솢거인 객체 제일 뒤에서 부터 -1을 시작으로 감소한다.

```python
obj =['g', 'i', 'l', 'l', 'o', 'g']

# Index References
'''
| g | i | l | l | o | g |
| 0 | 1 | 2 | 3 | 4 | 5 | # index 양수
|-6 |-5 |-4 |-3 |-2 |-1 | # index 음수
'''

print(obj[-2])
print(obj[4])

# o
# o
```


---
# slicing 예제

## obj[start :] : 특정 시작 위치 부터 끝까지 가져오기

```python
obj =['g', 'i', 'l', 'l', 'o', 'g']

print(obj[3:])
print(obj[-3:])

# ['l', 'o', 'g']
# ['l', 'o', 'g']
```

## obj[: end] : 시작부터 특정 종료 위치까지 가져오기

```python
obj =['g', 'i', 'l', 'l', 'o', 'g']

print(obj[:3])
print(obj[:-3])

# ['g', 'i', 'l']
# ['g', 'i', 'l']
```

## obj[start : end] : 특정 시작 위치부터 특정 종료 위치까지 가져오기

```python
obj =['g', 'i', 'l', 'l', 'o', 'g']

print(obj[2:4])
print(obj[-4:-2])

# ['l', 'l']
# ['l', 'l']
```

## obj[:: step] : step만큼 이동하면서 가져오기

```python
obj =['g', 'i', 'l', 'l', 'o', 'g']

# step 양수인 경우
print(obj[::2])
# ['g', 'l', 'o']

# end는 포함되지 않아 `g`는 포함되지 않음
print(obj[1:5:2])
# ['i', 'l']


# step 음수인 경우
print(obj[::-1])
# ['g', 'o', 'l', 'l', 'i', 'g']

print(obj[4::-2])
# ['o', 'l', 'g']
```