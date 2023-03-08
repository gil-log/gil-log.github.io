---
title: "[DB] MySQL NULL 처리(IFNULL, CASE, COALESCE)"
last_modified_at: 2020-11-23T22:21
categories: 
  - database
tags: 
  - 'COALESCE' 
  - 'case' 
  - 'db' 
  - 'ifnull' 
  - 'mysql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
**MySQL에서 Column의 값이 `Null`인 경우를 처리해주는 함수들은 `IFNULL`, `CASE`, `COALESCE`**과 같은 함수들이 있다.
_Orcale의 NVL()과 비슷한 기능을 한다._


# IFNULL

해당 **Column의 값이 NULL을 반환할 때, 다른 값으로 출력할 수 있도록 하는 함수**이다. 


- 기본 구조
```sql
SELECT IFNULL(Column명, "Null일 경우 대체 값") FROM 테이블명; 
```

- Example

```sql
// NAME Column이 NULL인 경우 "No name"을 출력, NULL이 아닌 경우 NAME Column을 출력
SELECT IFNULL(NAME, "No name") as NAME
FROM ANIMAL_INS
```

### IF()??

**Null 처리는 사실 `IF` 함수와 `IS NULL` 조건으로도 가능**하다.

- Example

```sql
// NAME Column이 NULL이 True인 경우 "No name"을, False인 경우는 NAME Column을 출력
SELECT IF(IS NULL(NAME), "No name", NAME) as NAME
FROM ANIMAL_INS
```

**!! MS-SQL의 `ISNULL()`과는 다르다.**

- Example
```sql
// MS-SQL인 상황, ISNULL() 예시
// NAME Column이 NULL인 경우 "No name"을, Null이 아닌 경우 NAME Column의 값을 출력
SELECT ISNULL(NAME, "No name") as NAME
FROM ANIMAL_INS
```
# CASE

**해당 Column 값을 조건식을 통해 True, False를 판단**하여 **조건에 맞게 Column값을 변환할 때 사용하는 함수**이다.


- 기본 구조
```sql
CASE 
    WHEN 조건식1 THEN 식1
    WHEN 조건식2 THEN 식2
    ...
    ELSE 조건에 맞는경우가 없는 경우 실행할 식
END
```

- Example

```sql
// NAME Column의 IS NULL 조건이 True인 경우 "No name" 출력
// WHEN 조건들에 True인 조건이 없을 경우 ELSE 문을 통해 NAME Column의 값 출력
// END 이후 그 Column의 별칭을 NAME으로 지정
SELECT 
    CASE
        WHEN NAME IS NULL THEN "No name"
        ELSE NAME
    END as NAME
FROM ANIMAL_INS
```


# COALESCE

**`COALESCE`**는 **지정한 표현식들 중에 NULL이 아닌 첫 번째 값을 반환**한다.
_**모든 DBMS에서 사용가능**_

표현식은 여러 항목 지정이 가능하고, 처음으로 만나는 NULL이 아닌 값을 출력한다.
_표현식이 모두 NULL일 경우엔 결과도 NULL 반환_

**`COALESCE`**는 배타적 OR 관계 열에서 활용도가 높다.
_엔터티(테이블)에서 두 개 이상의 속성(열) 중 하나의 값만 가지는 데이터 일 경우_

- 기본 구조
```sql
// NULL 처리 상황
SELECT COALESCE(Column명1, Column명1이 NULL인 경우 대체할 값)
FROM 테이블명


// 배타적 OR 관계 열
// Column1 ~ 4 중 NULL이 아닌 첫 번째 Column을 출력
SELECT COALESCE(Column명1, Column명2, Column명3, Column명4)
FROM 테이블명
```
- Example
```sql
// NAME Column의 값이 NULL인 경우 다음 표현식으로 넘어간다.
// 다음 표현식인 "No name"이 Null이 아니므로 "No name"을 출력.
SELECT COALESCE(NAME, "No name")
FROM ANIMAL_INS
```


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[COALESCE() 함수[김정선의 SQL Server 이야기]](https://m.blog.naver.com/PostView.nhn?blogId=visualdb&logNo=220362387072&proxyReferer=https:%2F%2Fwww.google.com%2F)

[[MySQL] CASE, COALESCE, IFNULL NULL 처리[exp_blog]](https://syujisu.tistory.com/173)

[ifnull, coalesce[Rough Stone :: Developer]](https://aterilio.tistory.com/45)

[[Mysql] NVL함수와 같은 기능을 하는 COALESCE ,IFNULL 함수[인큐]](http://blog.naver.com/PostView.nhn?blogId=software705&logNo=221315867526)

[]()

[]()