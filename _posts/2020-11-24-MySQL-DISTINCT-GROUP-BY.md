---
title: "[MySQL] DISTINCT, GROUP BY"
last_modified_at: 2020-11-24T22:41
categories: 
  - database
tags: 
  - 'db' 
  - 'distinct' 
  - 'group by' 
  - 'mysql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# DISTINCT

**`DISTINCT`는 중복되는 데이터 제거를 위해** 주로 **UNIQUE한 Column이나 Tuple(Record)를 조회하는 경우에 사용**한다.
**정렬(Filesort)하지 않고 결과를 출력해, GROUP BY에 비해 성능이 빠르다**.
_DISTINCT는 내부적으로 GROUP BY와 동일한 코드를 사용_
_DISTINCT는 "Grouping" 작업만, GROUP BY의  "Grouping" + "정렬" 작업_


- Example
```sql
// column1을 중복 제거 출력
SELECT DISTINCT column1 
FROM table;
```


# GROUP BY

**`GROUP BY`는 데이터를 Grouping해서, 집계 함수를 사용하고 그 결과를 정렬해 가져오는 경우에 사용**한다.
_주로 집계 함수 사용을 위해 사용한다._

**Grouping한 Column의 데이터만 가져오기때문에 출력 결과는 `DISTINCT`와 비슷**하다.

하지만 **`DISTINCT`의 출력 결과는 정렬된 결과가 아니지만, `GROUP BY`는 정렬된 결과**를 보내준다.

**정렬(Filesrot)작업을 하기 때문에 DISTINCT보다 성능이 느리다.**
_DISTINCT는 내부적으로 GROUP BY와 동일한 코드를 사용_

- example

```sql
SELECT column1 
FROM table 
GROUP BY column1;
```


**`GROUP BY`는 `HAVING`절을 통해 집계함수를 조건으로 사용 가능**하다.

## HAVING
**`WHERE`절 에서는 집계함수를 조건으로 사용할 수 없다.**

**그래서 `GROUP BY`에서 `HAVING`절을 추가하여 집계함수를 조건으로 사용하여 결과를 출력**한다.


```sql
// sal 평균이 1000 이상인 deptno와 sal 평균을 출력
SELECT deptno, AVG(sal)
FROM emp
GROUP BY deptno
HAVING AVG(sal) >= 1000;
```


# DISTINCT 와 GROUP BY

`DISTINCT`로 동시에 `GROUP BY`로도 처리될 수 있는 쿼리들이 있다.


**`DISTINCT`와 `GROUP BY` 차이**는 **`GROUP BY`는 "Grouping" + "정렬" 작업**을, 
**`DISTINCT`는 "Grouping" 작업만 수행하고 "정렬" 작업은 수행하지 않는 것**이다.

**`DISTINCT`와 `GROUP BY`는 일부 작업(Grouping)은 동일**하지만 **`GROUP BY`는 "정렬"을 하기 위한 부가적인 작업**을 더 하게 된다.
_"정렬"작업 이 필요하지 않다면 DISTINCT를 사용하는 것이 성능상 더 빠르다고 볼 수 있다._

하지만, **GROUP BY를 사용하는 경우에 정렬을 하지 않도록 유도할 수도 있다.**

explain을 사용해 GROUP BY 작업에 대한 실행 계획을 확인해보면 아래와 같다.

```sql
explain select * from user group by name ;      

+----+-------------+-------+------+..------+..------+---------------------------------+
| id | select_type | table | type |.. key  |.. rows | Extra                           |
+----+-------------+-------+------+..------+..------+---------------------------------+
|  1 | SIMPLE      | user  | ALL  |.. NULL |.. 5288 | Using temporary; Using filesort |
+----+-------------+-------+------+..------+..------+---------------------------------+
```
_user 테이블의 name 컬럼에는 인덱스가 없으므로 GROUP BY 시에 Temporary 테이블을 생성_

이때 **`GROUP BY` 절 뒤에 `ORDER BY NULL`을 붙혀주면 정렬(Filesort) 작업을 빼고 Grouping 작업만 실행** 한다.

실행 결과는 아래와 같다.

```sql
explain select * from user group by name order by null;

+----+-------------+-------+------+..------+..------+-----------------+
| id | select_type | table | type |.. key  |.. rows | Extra           |
+----+-------------+-------+------+..------+..------+-----------------+
|  1 | SIMPLE      | user  | ALL  |.. NULL |.. 5288 | Using temporary |
+----+-------------+-------+------+..------+..------+-----------------+
```
_MySQL에서만 특이하게 작동되는 방식, MySQL 개발사에 사용자들의 의견을 수렵하여 추가로 구현한 회피책_


<br>

`DISTINCT`와 `GROUP BY`는 정렬(Filesort)작업의 차이 외에도 각각 특색있게 사용 가능하다.


## DISTINCT로만 가능한 기능
```sql
// 이런 형태의 쿼리는 서브 쿼리를 사용하지 않으면 GROUP BY로는 작성하기 어렵다.
SELECT COUNT(DISTINCT column1) 
FROM table;
```


## GROUP BY로만 가능한 기능
```sql
// 집계 함수(Aggregation)가 필요한 경우에는 GROUP BY를 사용해야 한다.
SELECT column1, MIN(column2), MAX(column2) 
FROM table 
GROUP BY column1;
```




## !!DISTINCT는 함수가 아니다.
**`DISTINCT`는 함수가 아니라 키워드**이다. 
`DISTINCT`를 마치 함수인 것처럼 괄호를 사용하여 아래와 같은 상황에 사용하게 되면 결과를 출력할 수 없다.


```sql
// column1 컬럼은 unique 값, column2는 전체 값 출력을 원하지만
// column2 전체 값을 출력할 수 없다.
SELECT DISTINCT(column1), column2 
FROM table;
```


**`SELECT`절에 `DISTINCT`라는 키워드가 있으면**, **MySQL은 SELECT되는 모든 Column(Tuple)들에 대해서 `DISTINCT`를 적용해서 결과**를 출력한다.

따라서 **위 조건을 처리하기 위해서는 아래와 같이 GROUP BY로만 해결**할 수 있다.

```sql
SELECT column1, column2 
FROM table 
GROUP BY column1;
```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[mysql - DISTINCT 와 GROUP BY의 차이[linuxism]](https://linuxism.ustd.ip.or.kr/809)

[[Database] MySQL DISTINCT와 GROUP BY[inyong_pang.log]](https://velog.io/@inyong_pang/Database-MySQL-DISTINCT%EC%99%80-GROUP-BY)

[GROUP BY의 Filesort 작업 제거[into MySQL]](http://intomysql.blogspot.com/2010/12/group-by-filesort.html)

[]()

[]()

[]()