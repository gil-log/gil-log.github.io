---
title: "[MySQL] DATE_FORMAT"
last_modified_at: 2020-11-26T22:02
categories: 
  - database
tags: 
  - 'DATE_FORMAT' 
  - 'mysql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# DATE_FORMAT

**`DATE_FORMAT(DATETIME date, FORMAT)`**은 MySQL에서 **시간 타입의 Column**을 **원하는 FORMAT으로 변환하여 반환하는 함수**이다.

- Example
```sql
SELECT DATE_FORMAT(DATETIME, "%H"), COUNT(*)
FROM ANIMAL_OUTS
WHERE DATE_FORMAT(DATETIME, "%H")>=9 AND DATE_FORMAT(DATETIME, "%H")<20
GROUP BY DATE_FORMAT(DATETIME, "%H")
ORDER BY DATE_FORMAT(DATETIME, "%H")

// 출력 결과
DATE_FORMAT(DATETIME, "%H")	COUNT(*)
09	1
10	2
11	13
12	10
13	14
14	9
15	7
16	10
17	12
18	16
19	2

```

FORMAT에 올 수 있는 형태와 변환 결과는 아래와 같다.

|FORMAT|변환 결과|
|:--:|:--:|
|%M|월(Janeary, December, ...)|
|%m|월(01,02, ..., 12)|
|%D |월(1st, 2dn, 3rd, ...)|
|%c|월(1, 2, ..., 12)  |
|%b |월(Jan, Dec, ...) |
|%W|요일(Sunday, Monday, ...)|
|%a |요일(Sun, Tue, ...)|
|%w |요일(0, 1, 2) 0:일요일 |
|%d |일(00, 01, 02, ...)  |
|%e|일(0, 1, 2, ...) |
|%j|몇번째 일(120, 365) |
|%Y |연도(1987, 2000, 2013)|
|%y|연도(87, 00, 13) |
|%X |연도(1987, 2000) %V와 같이 쓰임.|
|%x |연도(1987, 2000) %v와 같이 쓰임.|
|%V|주(시작:일요일) 시작된 해의 몇번째 주인지 표시 (01-53) <br> 일요일이 주의 첫번째일 %X 와 함께사용|
|%v|주(시작:월요일) 시작된 해의 몇번째 주인지 표시 (01-53) <br> 월요일이 주의 첫번째일 %x 와 함께사용|
|%U |주(시작:일요일)| 
|%u|주(시작:월요일) |
|%H |시(00, 01, 02, 13, 24) |
|%h |시(01, 02, 12)|
|%I(대문자 아이)|시(01, 02, 12)|
|%k|시(0, 1, 2, ... ~ 23)|
|%l(소문자 엘)|시(1, 2, 12) |
|%i |분(00, 01, 30) |
|%r|"hh:mm:ss AM|PM" |
|%T |"hh:mm:ss" |
|%S|초 |
|%s |초 |
|%f|micro sec(100만분의 1초)|
|%p|AM, PM |


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[MySQL]DATE_FORMAT 날짜 표기[흘러간다...]](https://j07051.tistory.com/606)

[]()

[]()

[]()

[]()

[]()