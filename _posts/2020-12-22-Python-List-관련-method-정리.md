---
title: "[Python] List 관련 method 정리"
last_modified_at: 2020-12-22T13:48
categories: 
  - python
tags: 
  - 'List' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Python] 파이썬 자료형 : 리스트 (List) 생성 및 기본 사용법[R, Python 분석과 프로그래밍의 친구 (by R Friend)]](https://rfriend.tistory.com/329?category=695562)

[]()

[]()

[]()

[]()

[]()

<br>

# List

**`List` 자료형은 저장되는 데이터가 서로 다른 형태의 데이터여도 저장**되며,** 변경 가능하다는 점 때문에 데이터 분석에서 많이 사용**된다.
_튜플은 자료 갱신이 안된다._

---

# List Method

## del : 삭제

```python
list_1 = ['abc', 123, 3.14, ['edf', 456], ('gh', 'st')]

print(list_1)

# ['abc', 123, 3.14, ['edf', 456], ('gh', 'st')]

del list_1

print(list_1)
'''
Traceback (most recent call last):

  File "<stdin>", line 1, in <module>

NameError: name 'list_1' is not defined
'''
```

## 기본 연산자

**`리스트의 기본 연산자`**에는 **리스트 길이는 재는 `len()` 함수**, **리스트를 합치는 `+` 연산자**, **리스트 값을 반복하는 `*` 연산자, 포함 여부 값(True, False)을 반환하는 `in` 연산자, 함수를 반복하는데 사용하는 `for` loop 문 등**이 있다. 


|설명|Python|결과|
|:--:|:--:|:--:|
|길이|len([1, 2, 3])|3|
|리스트 합치기|[1, 2, 3] + [4, 5, 6]|[1, 2, 3, 4, 5, 6]|
|반복|[1, 2, 3] * 3|[1, 2, 3, 1, 2, 3, 1, 2, 3]|
|포함 여부|1 in [1, 2, 3]<br>4 in [1, 2, 3]|True<br>False|
|리스트 요소 반복|for i in [1, 2, 3]: print(i)|1<br>2<br>3|


## List 내장 함수

|함수|설명|
|:--:|:--:|
|len(list)|리스트 길이|
|max(list)|리스트 내 최대 요소|
|min(list)|리스트 내 최소 요소|
|list(seq)|리스트로 변환|

### len(list) : 리스트 길이

```python
list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

print(len(list_a))
# 5

print(len(list_b))
# 6
```

### max(list) : 리스트 내 최대 요소(문자인 경우 알파벳 순서 기준)

```python
list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

print(max(list_a))
# 5

print(max(list_b))
# o
```

### min(list) : 리스트 내 최소 요소(문자인 경우 알파벳 순서 기준)

```python

list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

print(min(list_a))
# 1

print(min(list_b))
# g
```
### list(seq) : 리스트로 변환

```python

tuple_a = ('gil', 'log')

print(type(tuple_a))
#<class 'tuple'>

list_tuple = list(tuple_a)

print(type(list_tuple))
#<class 'list'>

```

## Python List Method

### append(obj) : 기존 리스트에 1개 데이터 추가(2개 이상 불가)

```python
list_a = [1, 2, 3, 4, 5]

list_a.append(6)

print(list_a)
# [1, 2, 3, 4, 5, 6]
```

### extend(seq) : 기존 리스트에 다른 리스트 이어 붙이기

```python
list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

list_a.extend(list_b)

print(list_a)
# [1, 2, 3, 4, 5, 6, 'g', 'i', 'l', 'l', 'o', 'g']
```

### count(obj) : 리스트 안에 obj가 몇 개 들어 있는지 개수 반환

```python
list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

print(list_a.count(5))
# 1

print(list_b.count('l'))
# 2
```

### index(obj) : 리스트 에서 obj가 있는 가장 작은 index 반환

**list 안에 없는 obj를 인자로 넣고 실행 시키면 `ValueError`가 발생**한다.

```python
list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

print(list_a.index(4))
# 3

print(list_b.index('l'))
# 2
```

### insert(index, obj) : 리스트의 index 위치에 obj 값 삽입

```python
list_a = [1, 2, 3, 4, 5]

list_a.insert(3, 45)

print(list_a)
# [1, 2, 3, 45, 4, 5]
```

### pop() : 리스트의 마지막 요소 제거 및 반환

**인자로 index 위치 정수를 입력하면 해당 index의 요소를 제거 후 반환** 한다.


```python
list_a = [1, 2, 3, 4, 5]

print(list_a.pop())
# 5

print(list_a)
# [1, 2, 3, 4]

print(list_a.pop(2))
# 3

print(list_a)
# [1, 2, 4]
```

### remove(obj) : 리스트에서 obj 객체 제거(1개 인자만 가능, 2개 이상 TypeError 발생)


```python
list_b = ['g', 'i', 'l', 'l', 'o', 'g']

list_b.remove('l')
# ['g', 'i', 'l', 'o', 'g']
```


### reverse() : 리스트의 객체들 순서 반대로 뒤집기

```python
list_b = ['g', 'i', 'l', 'l', 'o', 'g']

list_b.reverse()

print(list_b)
# ['g', 'o', 'l', 'l', 'i', 'g']
```

### sort() : 리스트의 객체들 순서대로 정렬(오름차순)

**숫자와 문자가 섞여있는 list에서는 sort()를 실행 할 경우 `TypeError`가 발생**한다.

**한 종류로 이루어진 list에서만 실행 가능한 method** 이다.

**내림차순으로 정렬하고 싶다면 `sort(reverse=True)`로 인자에 `reverse=True`를 입력**해주면 된다.

```python
list_a = [1, 2, 3, 4, 5]

list_b = ['g', 'i', 'l', 'l', 'o', 'g']

# 파이썬 리스트 메소드

list_b.sort()

print(list_b)
# ['g', 'g', 'i', 'l', 'l', 'o']

list_a.sort(reverse=True)

print(list_a)
#[5, 4, 3, 2, 1]
```