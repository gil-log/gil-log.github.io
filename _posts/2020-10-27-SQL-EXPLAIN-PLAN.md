---
title: "[SQL] EXPLAIN PLAN"
last_modified_at: 2020-10-27T14:51
categories: 
  - database
tags: 
  - 'EXPLAIN PLAN' 
  - 'plan' 
  - 'sql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# EXPLAIN PLAN

EXPLAIN PLAN이란 SQL문의 액세스 경로를 확인하고 튜닝할 수 있도록 SQL문을 분석, 해석해 실행계획을 수립한 후 PLAN_TABLE에 저장하는 명령이다.

SQL문이 어떻게 실행되고 작동하는지를 점검하기 위해 사용한다.



# 사용방법

```
EXPLAIN PLAN[set statement_id ='1~30자 실행문 제목']
FOR sql statement
```

여기서 sql은 SELECT, INSERT, DELETE, UPDATE 등의 SQL 쿼리문을 말한다.

아래와 같이 EXPLAIN PLAN을 생성하고 나면

```
EXPLAIN PLAN FOR
SELECT e.EMPNO, d.DEPTNO
FROM EMP e, DEPT d
WHERE e.DEPTNO = d.DEPTNO;

```

다음과 같이 실행 계획을 확인할 수 있다.
```
SELECT *
FROM TABLE(dbms_xplan.display); cs
```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[오라클 - EXPLAIN PLAN(실행계획)](http://blog.naver.com/PostView.nhn?blogId=agopwns&logNo=221162719996&parentCategoryNo=&categoryNo=54&viewDate=&isShowPopularPosts=true&from=search)

[]()

[]()

[]()

[]()