---
title: "[Oracle] Package"
last_modified_at: 2021-11-25T01:20
categories: 
  - database
tags: 
  - 'oracle' 
  - 'package'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import

[[Oracle] 오라클 패키지 사용법 & 예제 (PL/SQL) [코딩팩토리]](https://coding-factory.tistory.com/456)
[Oracle PL/SQL 강좌 by 김정식[꿈꾸는 개발자, DBA 커뮤니티 구루비]](http://www.gurubee.net/lecture/1075)


---

# Package

**`Package`**는 **`Variable`, `Constants`, `Sub Programm(Procedure, Function)`의 집합체**로,

**`PL/SQL`에서 사용**하는 **`Procedure`, `Function`**를 **한 `Package`로 정의**하여,

**필요할 때 마다 `Package`안에 있는 `Procedure`, `Function`를 호출할 수 있게 해주는 집합체**이다.

_다른 Programming 언어에서 흔히 사용하는 Package와 동일하다고 생각하면 된다._

<br>

**`Package`**는 **여러 내부요소들**을,
_`Variable`, `Constants`, `Sub Programm(Procedure, Function)`_

모아 **하나의 `DBMS Object` 단위로 사용할 수 있게** 해주며,

**`Compile` 과정을 거쳐 `DB`에 저장**되어,

**다른 Programm**이나 **내부 Query에서 `Package`의 항목을 참조하여 실행**할 수 있다.


---

# Package 등장

**`Package`의 개념이 나오게 된건**,

**System이 거대해질 수록 사용하는 `Procedure`나 `Function`들**이,

**기하급수적으로 늘어**나게 되어 **관리가 까다로워지게 된다.**

**늘어난 여러 항목들**을 **비슷한 유형을 묶어 손 쉽게 관리하게 위해 등장**했다.


---

# Package 형식

**`Package`는 아래와 같이 `선언(SPEC)`, `본문(BODY)` 형식**을 지닌다.

```sql
// 선언(SPEC)
// 해당 Pacakge에서 사용할 항목들
// 변수, 커서, 예외 처리, Procedure, Function을 선언
// []는 생략 가능
// | 는 OR
CREATE [OR REPLACE] PACKAGE package_name 
IS | AS
    [ 변수 선언 절 ]
    [ 커서 선언 절 ]
    [ 예외 선언 절 ]
    [ Procedure, Function 선언 절 ]
END package_name;

// 본문(BODY)
// BODY 키워드로 해당 Package의 Procedure, Function의 수행 내용 부분을 정의
CREATE [OR REPLACE] PACKAGE BODY package_name 
IS | AS
    [ Procedure, Function 수행 절 ]
END package_name;
```
_**`IS | AS` 는 `Procedure`, `Package`에서는 동의어**로 동작_
_**`Table`, `View`, `Cursor`에서는 다른 의미**_
_**`AS`**는 **독립 실행형(Block, 하위 프로그램, Package 외부) Entity 용**으로 사용_
_**`IS`**는 **임베디드(Block, 서브 프로그램, Package 내부) Entity 용**으로 사용_

<br>

**선언 절**에서는 **해당 `Package`에 포함**될,
**`PL/SQL Procedure`, `Function`, `Cursor`, `Variable`, `Exception Hanlder` 부분을 선언**한다.


**`Package` 선언 절에서 선언한 모든 항목들**은 **해당 `Package` 전역으로 사용**할 수 있다.

<br>

**`Package` 본문, 수행 절에서는 `Procedure`, `Function` 호출 시 수행할 내용을 작성**한다.
_BODY 키워드 추가_



## Package 예제

아래는 Package 예제 코드이다.

```sql
CREATE OR REPLACE PACKAGE pack_gillog
IS
	
    FUNCTION func_gillog(P_MSG IN VARCHAR2)
    RETURN NULL;
    
    PROCEDURE proc_gillog;
    
END pack_gillog;

CREATE OR REPLACE PACKAGE BODY pack_gillog
IS

    FUNCTION func_gillog(P_MSG IN VARCHAR2)
    RETURN NULL
    IS
    BEGIN
    	 DBMS_OUTPUT.PUT_LINE(' FUNC MSG : ' || P_MSG);
         RETURN NULL;
    END func_gillog;
    
    PROCEDURE proc_gillog
    IS
    BEGIN
        DBMS_OUTPUT.PUT_LINE(' PROC EXE ');
    END proc_gillog;
    
END pack_gillog;
```

## Package 호출

**`Package`와 내부 요소들의 호출**은 **`package명.항목명` 형식으로 호출 가능**하다.


```sql
// DBMS_OUTPUT.PUT_LINE 출력 설정
SET SERVEROUTPUT ON;

// 패키지.함수 호출
EXEC pack_gillog.func_gillog('package test message');

// 패키지.프로시져 호출
EXEC pack_gillog.proc_gillog;

```