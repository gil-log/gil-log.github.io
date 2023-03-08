---
title: "[Oracle] Trigger"
last_modified_at: 2021-11-10T01:56
categories: 
  - database
tags: 
  - 'Low Level Trigger' 
  - 'Statement Level Trigger' 
  - 'TRIGGER' 
  - 'db' 
  - 'oracle'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[[ORACLE]오라클_트리거(TRIGGER)[나만의 기록들]](https://mine-it-record.tistory.com/107)

---

# Trigger

**`Trigger`**는 **DBMS에서 Data의 입력, 갱신, 삭제 등의 `Event`가 발생**할 때 마다,

**자동으로 수행되는 사용자 정의 `Procedure`**이다.

<br>

**`Procedure`나 `사용자 정의 함수(UDF)`는 사용자가 직접 호출**해야만 하지만, 

**`Trigger`는 사용자에게 알려주지 않고 자동으로 호출하여 처리**한다.



**`Trigger`는 `Table`과 별도로 DB에 저장**된다.

**`Trigger`는 `View`가 아닌 `Table`에 관해서만 정의**할 수 있다.

<br>

**일반적으로 `Trigger`**는 **SQL 제약 조건 방법을 통해 명시 불가능한 무결성 제약 조건을 구현**,

**관련 `Table` Data를 일치시킬 때 주로 사용**한다.

<br>

**제약 조건(Constraint)과 함께 데이터 무결성(Data Integrity)을 유지하는 방법**으로,

**특정 `Event`에 대해 자동으로 연속적 동작을 수행하는 형태**의 **저장 `Procedure`**라고 볼 수 있다.

---

# Trigger 종류

**`Trigger`는 `문장 트리거`, `행 트리거` 두 가지 종류**로 나눌 수 있다.

## Statement Level Trigger

**`문장 트리거(Statement Level Trigger)`**는 **`Trigger`가 설정된 `Table`에 `Trigger Event`가 발생**할 경우,

**많은 행(Row)에 대해 변경 작업이 발생하더라도**,

**오직 한번의 `Trigger`를 발생시키는 종류의 `Trigger`**이다.

**Column 값에 변화가 발생할 때마다 실행되는 방식**이다.


**`FOR EACH ROW` 옵션을 선언하지 않으면 `문장 트리거`로 선언**된다.


만약 **아래와 같은 SQL 문장이 실행되며 여러 행이 변경될 경우 단 한번만 실행**된다.

```sql
update TB_TEST
set log = log + 1;
```

## Row Level Trigger

**`행 트리거(Row Level Trigger)`는 조건을 만족하는 여러 개의 `Row`**에 대해,

**`Trigger`를 반복적으로 여러번 수행하는 방식**이다.

**`FOR EACH ROW` 옵션을 선언하면 `행 트리거`로 지정** 할 수 있다.

**`FOR EACH ROW WHEN 조건` 절로 정의**된다.

**Column Data 행에 변화**가 **올 때 마다 실행되는 방식**이다.

**변경 후 행을 `OLD`, `NEW`를 통해 가져 올 수 있다.**



---

# Trigger 제약

**`Trigger`는 트랜잭션 제어문(`COMMIT`, `ROLLBACK`, `SAVEPOINT`)를 사용할 수 없다.**

**`Trigger`는 `Triggering 문장`의 실행 부분**으로, **Triggering 문장과 같은 트랜잭션에 존재**한다.
_ Triggering 문장 : Trigger가 걸려 있는 대상 문장_

**`Triggering 문장`이 `COMMIT`, `ROLLBACK` 될 때**,

**`Trigger`의 작업 또한 `COMMIT`, `ROLLBACK` **된다.


---

# Trigger 형식

**`Trigger`는 기본적으로 아래와 같은 형식**을 지닌다.

```sql
CREATE [OR REPLACE] TRIGGER `트리거 명`
BEFORE | AFTER
[INSERT, UPDATE, DELETE(동작)] ON `테이블 명`
[REFERENCING NEW | OLD TABLE AS `테이블 명`]
[FOR EACH ROW]
[WHEN 조건식]

Trigger Body
```
**위 형식에서 사용된 옵션들의 설명은 아래**와 같다.

|옵션|설명|
|:--:|:--:|
|OR REPLACE|생성할 Trigger와 같은 이름을 가지고 있어도, 무시하고 새로운 것으로 갱신|
|BEFORE| TABLE이 변경되기 전 Trigger가 실행|
|AFTER|TABLE이 변경된 후 Trigger가 실행|
|동작|INSERT, UPDATE, DELETE 등 해당 동작이 실행 될 때 Trigger가 실행|
|NEW| 새로 추가, 변경 된 후의 값에 Trigger가 적용<br>_INSERT:입력하려는 값, UPDATE:수정 하려는 값_|
|OLD| 변경 되기 전의 값에 Trigger가 적용<br>_UPDATE:수정 전 값, DELETE:삭제할 값_|
|FOR EACH ROW|행 트리거로 설정 한다.|
|WHEN|Trigger가 실행 될 때 특정 조건 Data에만 Trigger를 지정하는 조건 값|
|Trigger Body| 트리거 본문 코드 입력 부분<br> BEGIN으로 시작, END로 끝날 때 하나 이상의 SQL문이 존재해야 한다.<br>변수에 값을 치환할 때 예약어 SET 사용|

---

## Trigger 예제

```sql
CREATE OR REPLACE TRIGGER gillog_triger
       BEFORE
       INSERT, UPDATE ON `TB_GILLOG`
       FOR EACH ROW
       BEGIN
        DBMS_OUTPUT.PUT_LINE('변경 전 타이틀 : ' || : old.title);
        DBMS_OUTPUT.PUT_LINE('변경 후 타이틀 : ' || : new.title);
       END;

// DBMS_OUTPUT.PUT_LINE을 출력 설정
SQL> SET SERVEROUTPUT ON ; 

// UPDATE문 실행
SQL> UPDATE TB_GILLOG SET title = '211110 Trigger 길로그' WHERE logid = 1412

// Trigger가 자동 실행 결과가 출력 
변경 전 타이틀 : 2a326864-ee60
변경 후 타이틀 : 211110 Trigger 길로그

1 행이 갱신되었습니다.
```


---

# Trigger 관리


## Trigger 조회

```sql
SELECT *
FROM USER_TRIGGERS
```

## 활성화 / 비활성화

```sql
ALTER TRIGGER `Trigger 명` [ENABLE|DISABLE]
```


## 테이블에 속한 Trigger 활성화 / 비활성화

```sql
ALTER TABLE `Table 명` [ENABLE|DISABLE] ALL TRIGGER
```


## Trigger 수정 후 재 컴파일

```sql
ALTER TRIGGER `Trigger 명` COMPILE;
```


## Trigger 삭제

```sql
DROP TRIGGER `Trigger 명`
```
