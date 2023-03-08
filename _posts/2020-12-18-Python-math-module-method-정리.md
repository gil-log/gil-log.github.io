---
title: "[Python] math module 정리"
last_modified_at: 2020-12-18T02:31
categories: 
  - python
tags: 
  - 'math' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[핵심만 간단히, Hello World! 파이썬 3](https://wikidocs.net/book/1657)

[[Python] Math Library[WildTurtle]](https://m.blog.naver.com/PostView.nhn?blogId=josing616&logNo=220735048027&proxyReferer=https:%2F%2Fwww.google.com%2F)



[]()

[]()

[]()

[]()

<br>


# math

**`math` module은 단순 python 연산을 넘어 조금 더 복잡한 산술 연산이 필요할 때 사용하는 module**이다.

`import math`를 통해서 `math` module 을 사용할 수 있다.


## constant

### math.pi
원주율 파이로, 3.141592653589793이다. 

### math.e
자연 상수 e로, 2.718281828459045이다. 

### math.tau

타우 상수로, 6.283185307179586이다. 

## method

`math` module이 지원하는 method들의 간단 정리 표는 아래와 같다.

|함수|설명 |
|:--:|:--:|
|math.pi|원주율 |
| math.e|자연상수 |
|abs()|절대값 계산 함수(내장함수)|
|round()|반올림 계산 함수(내장함수)|
 |math.trunc()|버림 계산 함수 |
 |math.factorial()|팩토리얼 계산 함수 |
 |math.degrees()|라디안을 입력받아 도를 계산 |
 |math.radians()|도를 입력받아 라디안을 계산 |
 |math.cos()|입력된 라디안에 대한 코사인 값을 계산 |
 |math.sin()|입력된 라디안에 대한 사인 값을 계산|
 |math.tan()|입력된 라디안에 대한 탄젠트 값을 계산 |
 |math.acos()|cos()의 역함수 |
 |math.asin()|sin()의 역함수 |
 |math.atan()|tan()의 역함수 |
|math.pow()|제곱 연산 |
|math.sqrt()|제곱근 연산 |
|math.log()|첫 번째 매개변수의 로그를 구합니다.|
|math.log10()|밑수가 10인 로그를 계산합니다.|



### math.pow(3, 2)

**`math.pow(x, y)`는 x에 y 승을 계산한 결괏값을 반환**한다.

```python
math.pow(3, 2)
# 9
```
### math.sqrt(25)

**주어진 인자의 제곱근의 값을 반환**한다.

```python
math.sqrt(25)
# 5.0
```

### math.ceil(3.14)

주어진 인자의 소수점을 올림하여 정수로 변환하는 함수이다.

```python
math.ceil(3.14)
# 4
```
### math.floor(3.78)

주어진 인자의 소숫점을 내림하여 정수로 변환하는 함수이다.

**`math.floor()`는 무조건 내림 처리** 한다.

```python
math.floor(3.78)
#3
```

### math.trunc(3.14)
주어진 인자의 소숫점을 내림하여 정수로 변환하는 함수이다. 

`math.floor()`와 비슷해 보이지만 `math.trunc()`는 0으로 향하여 내린다.

```python
math.trunc(-3.14)
# -3
math.floor(-3.14)
# -4
```

### math.copysign(3.14, -3)

두 번째 인자의 부호를 가져와 첫 번째 인자의 부호에 적용하는 함수이다.

```python
math.copysign(3.14, -3)
# -3.14
```

### math.fabs(-3.14)

절댓값을 반환하는 함수이다.

```python
math.fabs(-3.14)
# 3.14
```

### math.factorial(5)

팩토리얼 함수로써, 1부터 주어진 인자의 값까지 모두 곱한 값을 반환한다.

```python
math.factorial(5)
# 1*2*3*4*5 = 120
```

### math.frexp(100) 

입력 받은 인자의 값과 동일하게 연산 가능한 `m * 2^e`과 같은 값을 가지는 m, e를 반환하는 함수이다.

```python
math.frexp(100) 
# (0.78125, 7)
# 0.78125*2**7 = 100
```

### math.ldexp(0.78125, 7)
`math.frexp()`의 반대 함수로써, `m * 2^e`에 각각 대입, 계산한 값을 반환한다. 

**m이 첫 번째 인자이고 e가 두 번째 인자**이다.

```python
math.ldexp(0.78125, 7)
# 100
```

### math.gcd(6, 8)
두 수의 최대 공약수를 반환한다.

```python
math.gcd(6, 8)
#2
```

### math.modf(3.14)
**`math.modf()` 함수는 입력값을 정수와 소수 부분으로 분리해 반환**한다. 

`math.modf()`함수는 부동소수점의 값을 그대로 반환한다.

```python
math.modf(3.14)
# (0.14000000000000012, 3.0)
```

위 경우 소수가 0.14가 아니라 긴 소수 값이 출력된다. 

이는 **부동소수점 인해 발생한 문제**로, **부동소수점은 10진법 수를 2진법 체계에서 정확히 반영하지 못해 생기는 문제**이다.

### math.log(10, 10)
**`math.log(a, b)`는 로그 함수이며 b를 밑으로 하는 log a에 대한 로그 값을 리턴**한다.

```python
math.log(10, 10)
# 1
```

### math.log1p(x)

e를 밑으로 하는 x+1로그 값을 반환 한다.

### math.log2(x)

2를 밑으로 하는 x로그 값을 반환 한다.

### math.log10()
10을 밑으로 하는 x로그 값을 반환 한다.


### math.degrees(x)
라디안으로 표현된 각도를 60분법 각도로 변환하는 함수이다.

### math.radians(x)
60분법으로 표현된 각도를 라디안 각도로 변환하는 함수이다.

