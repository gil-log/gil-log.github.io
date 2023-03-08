---
title: "[Python] Formatting(포매팅)"
last_modified_at: 2020-12-25T12:57
categories: 
  - python
tags: 
  - 'formatting' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Practical Python Programming - 2.3 포매팅](https://wikidocs.net/84390)

[]()

[]()

[]()

[]()

[]()

<br>

# Formatting

아래와 같이 **데이터를 작업할 때 구조화된 출력(테이블 등)이 필요할 때**가 있다.

**`Formatting`은 데이터들을 정형화 시킬때 사용**된다.

```
      Name      Shares        Price
----------  ----------  -----------
        AA         100        32.20
       IBM          50        91.10
       CAT         150        83.44
      MSFT         200        51.23
        GE          95        40.37
      MSFT          50        65.10
       IBM         100        70.44
```



## 문자열 포매팅

**Python 3.6 이상에서는 문자열 포매팅에 `f 문자열(f-string)`을 사용할 수 있다.**

`{표현식:포맷}` 부분이 대체된다.

```python
name = 'IBM'
shares = 100
price = 91.1
print(f'{name:>10s} {shares:>10d} {price:>10.2f}')
# '       IBM        100      91.10'
```

## 포맷 코드

`포맷 코드({} 내의 : 이후)`는 `C`의 `printf()`와 비슷하다. 

일반적인 포맷 코드는 다음과 같다.

```
d       십진 정수
b       이진 정수
x       16진 정수
f       부동소수점수 [-]m.dddddd
e       부동소수점수 [-]m.dddddde+-xx
g       E 표기를 선택적으로 사용하는 부동소수점
s       문자열
c       문자(정수로부터)
```
필드 폭과 정밀도를 조정하는 공통 수정자 포맷 코드는 아래와 같다.
```
:>10d   정수를 10자리 필드에 오른쪽 정렬
:<10d   정수를 10자리 필드에 왼쪽 정렬
:^10d   정수를 10자리 필드에 가운데 정렬
:0.2f   부동소수점수를 두 자리 정밀도로 나타냄
```

## 딕셔너리 포매팅


**`format_map()` 메서드를 사용해 값들의 딕셔너리에 문자열 포매팅을 적용**할 수 있다.

**f 문자열과 같은 코드를 사용**하되, **제공된 딕셔너리로부터 포맷을 적용**한다.

```python
s = {
    'name': 'IBM',
    'shares': 100,
    'price': 91.1
}

print('{name:>10s} {shares:10d} {price:10.2f}'.format_map(s))
# '       IBM        100      91.10'
```

## format() method

**`format()` 메서드는 `인자` 또는 `키워드 인자`에 대한 포매팅을 적용**할 수 있다.

**코드 가독성 문제로 추천되는 방식은 아니다.**

```python
print('{name:>10s} {shares:10d} {price:10.2f}'.format(name='IBM', shares=100, price=91.1))
# '       IBM        100      91.10'

print('{:10s} {:10d} {:10.2f}'.format('IBM', 100, 91.1))
# '       IBM        100      91.10'
```

## C 스타일 포매팅

**포매팅 연산자 `%`를 사용할 수도 있다.**

이때 **오른쪽에는 단일 항목 또는 튜플이 있어야 한다. **

**포맷 코드는 `C`의 `printf()`와 유사**하다.
```python
print('The value is %d' % 3)
# 'The value is 3'

print('%5d %-5d %10d' % (3,4,5))
# '    3 4              5'

print('%0.2f' % (3.1415926,))
# '3.14'
```


**아래는 바이트 문자열에 대해서만 사용 가능한 포매팅**이다.

```python
print(b'%s has %d messages' % (b'Dave', 37))
# b'Dave has 37 messages'
```