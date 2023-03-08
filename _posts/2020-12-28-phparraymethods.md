---
title: "[PHP] Array methods"
last_modified_at: 2020-12-28T10:56
categories: 
  - php
tags: 
  - 'array' 
  - 'php'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[PHP 배열 [정보통신기술용어해설]](http://www.ktword.co.kr/abbr_view.php?m_temp1=5726)

[]()

[]()

[]()

[]()

[]()

<br>

---

# Array

**PHP에서 Array는 `key`, `value` 형태로 index에 숫자 이외에도 `key`값을 설정할 수 있다.**

**`Java`에서 Map이나, `Python`에서 Dictionary와 같은 기능**을 **`PHP`에서는 Array로도 기능**할 수 있다.


---
## Array 생성

**1. `array()`를 이용해 생성** 할 수 있다.

```php
$array = array( , , , , , , );
```



**2. `PHP 5.4`이후 버전 부터 `$array = [ , , , , , ]; `와 같이 단축 문법으로도 생성 가능**하다.

---
## Array Basic Function

|함수|기능|
|:--:|:--:|
|is_array()|배열 여부 판별|
|count()|배열 개수|
|sizeof()|배열 개수|
|array_count_values()|배열 개수|
|each()|배열 키, 값 한 쌍 반환|
|list(변수1, 변수2, ..) = $array;|배열 내 값들 각 변수에 저장|

---



## Array 출력

### print_r($array)
Array 변수를 알기 쉬운 형태로 출력한다.

### var_dump($array)
`print_r()`보다 더 많은 정보(배열 크기 등)를 출력한다.

### var_export($array)
`PHP Script`로 그대로 가져다 사용할 수 있도록 출력한다.

---
## Array 순회

```php
foreach($array as $value)

foreach($array as $key => $value)
```
---
## Array 비교

### array_intersect($first, $second)
같은 원소들만의 `Array`로 반환한다.

### array_diff($first, $second)
다른 원소들만의 `Array`로 반환한다.

---

## Array 검색

### array_key_exists(key, search)
주어진 key로 검색한 후 존재 여부를 반환 한다.

### in_array(needle, haystack)
주어진 값으로 검색한 후 존재 여부를 반환 한다.

### array_search(needle, haystack)
주어진 값으로 검색한 후 존재할 시 해당 키를 반환한다.

존재하지 않을 경우 false를 반환 한다.

---


## Array 결합

### array_merge($array1, $array2)

**`$array1`에 `$array2`을 추가**한다.

**결합 결과는 key값이 0부터 다시 책정** 된다.

**숫자키가 중복될 경우 덮어쓰지 않게 배열되고 순서대로 결합**된다.

**문자키가 중복될 경우 뒤 배열이 앞 배열 원소를 덮어 씌운다.**

### $array1 + $array2

`$array1`과 `$array2`를 index별로 결합한다.

key가 중복될 경우 `$array2`가 `$array1`의 원소를 덮어 씌운다.

---


## Array 원소 추가, 삭제

### array_unshift()

시작점에 추가한다. 추가 원소 키는 0이며 나머지 키들의 일련번호가 다시 매겨진다.

### array_shift()

시작점의 원소를 추출한다.


### $array[] = 값 or array_push()
끝점에 원소를 추가한다.

### array_pop()
끝점에 원소를 추출한다.

### array_splice($array, 추가 위치, 0 , 추가 값)

결과 키는 다시 매겨 진다.


----

## Array 정렬



## 값에 의한 정렬

### sort($array [,비교방법])

오름차순으로 정렬한다. 숫자키의 경우 키값의 순서가 초기화 된다.

### rsort($array [,비교방법])

내림차순으로 정렬한다. 숫자키의 경우 키값의 순서가 초기화 된다.

### asort($array [,비교방법])
오름차순으로 정렬한다. 배열의 키 값을 그대로 유지 한다.

### arsort($array [,비교방법])

내림차순으로 정렬한다. 배열의 키 값을 그대로 유지 한다.


## 키에 의한 정렬

### ksort($array [,비교방법])

오름차순으로 정렬한다.

### krsort($array [,비교방법])

내림차순으로 정렬한다.

### ![,비교방법]

SORT_NUMBER(숫자로 비교), SORT_STRING(문자열로 비교)
인수로 전달된 $array가 직접 정렬 된다.
성공할 경우 success, 실패시 false를 반환한다.



## 사용자 정의 비교 함수로 정렬


### usort($array, 사용자 정의 비교 함수)

### uksort($array, 사용자 정의 비교 함수)

### uasort($array, 사용자 정의 비교 함수)

사용자가 정의한 비교 함수에 의해 정렬하며 Array의 키 값은 그대로 유지한다.

2차원 이상의 Array도 정렬 가능하다.


## 역방향 정렬

### array_reverse($array, [,true/false])

Array를 역방향으로 정렬한다.

**`$array`를 역방향으로 정렬한 배열을 생성한 후 그 새 배열을 리턴하는 방식**이다.


정렬 수행 후 결과 키는 순서대로 숫자가 초기화 되어 변경된다.

매개 변수 두 번째 인자를 `true`로 해두면 원래 키를 그대로 유지하며 역방향 정렬한다.


문자 키의 경우 두 번째 인자에 관계없이 문자 그대로 반영 된다.


## 무작위 정렬
### shuffle($array)

---
