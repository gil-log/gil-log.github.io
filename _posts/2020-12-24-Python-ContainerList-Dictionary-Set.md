---
title: "[Python] Container(List, Dictionary, Set)"
last_modified_at: 2020-12-24T08:56
categories: 
  - python
tags: 
  - 'List' 
  - 'container' 
  - 'dictionary' 
  - 'python' 
  - 'set'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Practical Python Programming - 2.2 컨테이너](https://wikidocs.net/84389)

[]()

[]()

[]()

[]()

[]()

<br>

---

# Container

Python에서 **`Container`란 자료형(Data type)의 저장 모델로 종류에 무관하게 데이터를 저장할 수 있음을 의미**한다.

**`String`, `Tuple`, `List`, `Dictionary`, `Set` 등은 종류에 무관(Container)한 형식**이며, **정수, 실수, 복소수등은 단일 종류(Literal)한 형식**이다.

---

# In Programming Using Container

**Program에서 많은 수의 객체를 다뤄야할 때**가 있다.

- 주식 포트폴리오

- 주식 가격 테이블

이때 **이 객체들을 사용할때 세 가지 선택사항**이 있다.

1. **`List`**
**순서가 유지되는(ordered) 데이터**.

2. **`Dictionary`**
**순서가 없는(unordered) 데이터**.

3. **`Set`**
**고유한 항목의 집합**이며 **순서가 없음**.



---

# List As Container

**`List`는 데이터의 순서가 중요할 때는 사용**한다.

**`List`는 어떤 유형의 객체든 담을 수 있다.**
_예: 튜플의 리스트._

```python
portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.3),
    ('CAT', 150, 83.44)
]

portfolio[0]
# ('GOOG', 100, 490.1)

portfolio[2]
# ('CAT', 150, 83.44)

```

## List Construction

다음은 리스트에 항목을 하나하나 채워넣는 예이다.

```python
records = []  # 빈 리스트로 초기화

# .append()를 사용해 항목을 추가
records.append(('GOOG', 100, 490.10))
records.append(('IBM', 50, 91.3))
...
```
다음은 파일에서 레코드를 읽어 리스트에 채우는 예이다.

```python
records = []  # 빈 리스트로 초기화

with open('Data/portfolio.csv', 'rt') as f:
    next(f) # 헤더를 건너뜀
    for line in f:
        row = line.split(',')
        records.append((row[0], int(row[1]), float(row[2])))
```

---

# Dictionary As Container

**`Dictionary`는 빠른 임의 조회(키 이름을 사용)에 유용**하다. 
_예: 주식 가격의 딕셔너리_

```python
prices = {
   'GOOG': 513.25,
   'CAT': 87.22,
   'IBM': 93.37,
   'MSFT': 44.12
}

prices['IBM']
# 93.37

prices['GOOG']
# 513.25
```

## Dictionary Construction

다음은 `Dictionary`에 항목을 하나하나 채우는 예이다.

```python
prices = {} # 빈 딕셔너리로 초기화

# 새 항목을 삽입
prices['GOOG'] = 513.25
prices['CAT'] = 87.22
prices['IBM'] = 93.37
```

다음은 파일 내용으로부터 `Dictionary`를 채우는 예이다.

```python
prices = {} # 빈 딕셔너리로 초기화

with open('Data/prices.csv', 'rt') as f:
    for line in f:
        row = line.split(',')
        prices[row[0]] = float(row[1])
```

## Dictionary 조회

키가 존재하는지 테스트할 수 있다.
```python
if key in d:
    # YES
else:
    # NO
```
존재하지 않는 값을 찾으려 할 경우 기본값을 제공할 수 있다.
```python
name = d.get(key, default)


prices.get('IBM', 0.0)
# 93.37
prices.get('SCOX', 0.0)
# 0.0
```


## Composite Keys(복합 키)

**Python에서는 어떤 타입의 값이든 `Dictionary`의 키로 사용할 수 있다.**

**`Dictionary`의 키는 반드시 변경불가능한(Immutable) 타입**이어야 한다.

**`List`, `Set`, 다른 `Dictionary`는 변경 가능(Mutable)**하므로 **딕셔너리의 키로 사용할 수 없다.**

```python
# 튜플의 예:

holidays = {
  (1, 1) : 'New Years',
  (3, 14) : 'Pi day',
  (9, 13) : "Programmer's day",
}

holidays[3, 14]
# 'Pi day'
```
---

# Set As Container

**`Set`는 고유 항목의 모음**이며 **순서를 유지하지 않는다.**

```python
tech_stocks = { 'IBM','AAPL','MSFT' }
# 대체 구문
tech_stocks = set(['IBM', 'AAPL', 'MSFT'])
```

`Set`는 멤버십 테스트에 유용하다.

```python
>>> tech_stocks
set(['AAPL', 'IBM', 'MSFT'])
>>> 'IBM' in tech_stocks
True
>>> 'FB' in tech_stocks
False
>>>
```

`Set`는 중복 제거에도 유용하다.

```python
names = ['IBM', 'AAPL', 'GOOG', 'IBM', 'GOOG', 'YHOO']

unique = set(names)
# unique = set(['IBM', 'AAPL','GOOG','YHOO'])
```

## ETC Set 연산

```python
names.add('CAT')        
# 항목을 추가

names.remove('YHOO')    
# 항목을 제거

s1 | s2                 
# 합집합

s1 & s2                 
# 교집합

s1 - s2                 
# 차집합
```