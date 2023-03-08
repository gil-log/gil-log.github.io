---
title: "[MySQL] SELECT된 결과로 INSERT 하기"
last_modified_at: 2021-04-29T06:27
categories: 
  - database
tags: 
  - 'INSERT' 
  - 'migration' 
  - 'mysql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[SQL SELECT 결과를 INSERT 하기[ZETAWIKI]](https://zetawiki.com/wiki/SQL_SELECT_%EA%B2%B0%EA%B3%BC%EB%A5%BC_INSERT_%ED%95%98%EA%B8%B0)

[]()

[]()

[]()

[]()

[]()

<br>

# SELECT 결과 INSERT 하기


**SELECT된 결과를 가지고 INSERT 쿼리를 실행하는 문법**은 아래와 같다.



## All Column

**모든 컬럼을 가지고 조회된 결과를 INSERT하는 문법**은 아래와 같다.

```sql
INSERT INTO 들어갈테이블명
SELECT * FROM 조회할테이블명
```


## Some Column

**일부 컬럼을 가지고 조회된 결과를 INSERT하는 문법**은 아래와 같다.

```sql
INSERT INTO 들어갈테이블명
(컬럼명1, 컬럼명2, 컬럼명3)
SELECT 컬럼명1, 컬럼명2, 컬럼명3
FROM 조회할테이블명
```

**아래는 실제 사용 예시**이다.

```sql
INSERT INTO GILLOG (work_id)
  SELECT HOMEWORK.work_id
  FROM HOMEWORK WHERE HOMEWORK.isDone = 0;
```