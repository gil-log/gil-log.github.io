---
title: "Transaction Isolation Level"
last_modified_at: 2021-05-24T00:56
categories:
  - concept
tags: 
  - 'Nonrepeatable Read' 
  - 'dirty read' 
  - 'isolation level' 
  - 'phantom read' 
  - 'transaction'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[SQL 트랜잭션 - 믿는 도끼에 발등 찍힌다[👷 The Sapzil]](https://blog.sapzil.org/2017/04/01/do-not-trust-sql-transaction/)

[Transaction[개발자 이준스]](https://juns-lee.tistory.com/m/entry/Transaction?category=830260)

---

# Transaction Problem


**`Transaction`이 문제가 생길 수 있는 경우는 아래 가난한 자를 부자로 만들어주는 `setPoorToRichEvent` 비즈니스 로직 부분을 따라가면 확인**할 수 있다.

```java
private PlatformTransactionManager transactionManager;

public void setTransactionManager(PlatformTransactionManager transactionManager){
    this.transactionManager = transactionManager;
}

public setPoorToRichEvent(Account account) throws Exception {
    TransactionStatus status =
    	this.transactionManager.getTransaction(new DefaultTransactionDefinition());

    try {
    	//SELECT state FROM event WHERE id = ?
        String state = paymentDao.getAccountSate(account);
        if(state.equals("poor")) {
            //UPDATE event SET state = 'rich' WHERE id = ?
            paymentDao.setEventState(account);
            //UPDATE account SET  money = money * 100 WHERE id = ?
            paymentDao.setAccountHundredEvent(account);
        }
        this.transactionManager.commit(status);
    }
    catch(Exception e) {
        this.transactionManager.rollback(status);
        throw e;
    }
```

**`setPoorToRichEvent` 비즈니스 로직**은, **DB에서 id값을 가지고 account(계좌)의 state(상태 값)을 가져온 후, state가 `poor`인 회원**을 **`rich`로 변경**하고 **계좌를 100배** 늘려주는 **은행사의 일생일대 Event를 수행하는 비즈니스 로직**이다.

**`Transaction` 까지 생각하여 문제가 없을 거라 생각했던 해당 로직**은 **아래와 같은 상황에서 문제가 발생**한다.


|Transaction A	|Transaction B|
|:--:|:--:|
|BEGIN	 ||
|SELECT state FROM account WHERE id = 1	 ||
| 	|BEGIN|
| 	|SELECT state FROM account WHERE id = 1|
| UPDATE event SET state = 'rich' WHERE id = 1	|UPDATE event SET state = 'rich' WHERE id = 1|
||UPDATE account SET  money = money * 100 WHERE id = 1|
|	|COMMIT|
|UPDATE account SET  money = money * 100 WHERE id = 1||
|COMMIT	 ||

>_before money = 100_
respected money = 10,000
_after money = 1,000,000_


이러한 상황은 왜 발생한 것일까?

**`setPoorToRichEvent` 비즈니스 로직**은 **event Table의 `state`가 `poor`인 경우 event Table의 `state`를 `rich`로 변경**하고,** account Table의 `money`를 100배 늘려주는 로직**이다.

**`Transaction B` 기준**에서 **`Transaction A`에서 event Table의 `state`를 `rich`로 변경하는 로직 부분의 수행이 늦어져** event Table의 **`state`를 읽어왔을때 `poor` 값**이었고 **동일 로직이 똑같이 수행**되었던 것이다.

그래서 id 1번 유저는 **한번만 참여 가능한 이벤트를 중복 참여**하게 되어 **계좌가 100배가 아닌 10,000배가 늘어나게 되었다.**


<br>


**이러한 상황이 발생한 것은 `Isolation Level` 격리 수준과 관련**있다.

**기본 `isolation level`**에서 **UPDATE 쿼리**는 대상 레코드를** 다른 `Transaction`이 먼저 업데이트한 뒤 `commit`된 경우 업데이트 된 레코드를 가지고 쿼리를 수행**하게 된다.

>… a target row might have already been updated (or deleted or locked) by another concurrent transaction by the time it is found. In this case, the would-be updater will wait for the first updating transaction to commit or roll back (if it is still in progress). … If the first updater commits, the second updater … **will attempt to apply its operation to the updated version of the row. (Postgres 문서)**
The snapshot of the database state applies to SELECT statements within a transaction, not necessarily to DML statements. If you insert or modify some rows and then commit that transaction, a DELETE or UPDATE statement issued from another concurrent REPEATABLE READ transaction could affect those just-committed rows, even though the session could not query them. (MySQL 문서)


**`isolation level`과 관련되어 발생할 수 있는 문제들은 아래**를 살펴보면 알 수 있다.



---

# Isolation level Level

**SQL 표준에서 `Isolation level`**은 **`READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE` 네 가지**이다. 


| isolation level | 의미 |
|:--:|:--:|
|READ_UNCOMMITTED|**commit 되지 않은 데이터 변경사항**을 읽을 수 있다.|
|READ_COMMITTED| **commit된 데이터 변경사항만** 읽을 수 있다.|
|REPREATABLE_READ|**Transaction이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 `shared lock`**이 걸리므로 **다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능**하다.<br>**선행 Transaction이 읽은 Data**는 **Transaction이 종료될 때까지 후행 Transaction이 갱신하거나 삭제하는 것을 불허함**으로써 **같은 Data 레코드는 여러번 반복해서 읽더라도 동일한 값**을 읽도록 한다.<br>**자신이 변경한 레코드는 변경된 값을 읽게 된다.**|
|SERIALIZABLE|**Transaction이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 `shared lock`**이 걸리므로 **다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능**하다.<br>**완전한 ACID를 보장하는 격리 수준**이지만,  **가장 성능이 비효율적인 격리 수준**이다.|


아래는 **DBMS 별 기본 Transaction Isolation level을 정리한 표**이다.

|DBMS|Isolation level Default|
|:--:|:--:|
|Oracle|READ COMMITTED|
|MySql|REPEATABLE READ (Inno DB)|
|Mssql|READ COMMITTED|



**`SERIALIZABLE`이 가장 높은 격리수준**이지만 **성능 상의 이유**로 **MySQL (InnoDB)은 `REPEATABLE READ`** 기본값이다.



**각 격리 수준에 따라 아래와 같은 문제가 발생** 할 수 있는데 **`Dirty Read`, `Nonrepeatable Read`, `Phantom Read` 이렇게 세 가지 문제가 발생**할 수 있다.

---

## Dirty Read

**`Dirty Read`**는 **한 `Transaction`에서 다른 `Transaction`에 의해 변경**되었지만 **아직 `commit`되지 않은 데이터를 읽게 되는 문제**이다.

이 Data가 **`commit`되지 않고 `rollback`**되어 버린다면, **첫 번째 `Transaction`**에서 읽은 **이 Data는 유효하지 않은 Data**가 된다.

 

## Nonrepeatable Read

**`Nonrepeatable Read`**는 **`Transaction`이 같은 질의를 두 번 이상 수행**할 때 **서로 다른 Data를 얻게 되는 문제**이다.

보통 **각 질의 수행 사이에 동시 진행 중인 다른 `Transaction`에서 이 Data를 변경하는 경우에 발생**한다.

 

## Phantom Read

**`Phantom Read`**는 **어떤 `Transaction A`에서 둘 이상의 Data 행을 읽은 다음**, **동시 진행 중인 다른 `Transaction B`**가 **추가 행을 삽입할 때 발생**한다.

**`Transaction A`에서 동일한 질의를 다시 수행**하면, `Transaction A`는 **이전에 없던 Data 행까지 읽게된다.**


---

아래는 **`Isolation Level` 별**로 위 세 가지 **`Dirty Read`, `Nonrepeatable Read`, `Phantom Read`가 발생할 수 있는 경우**를 정리한 표이다.


| isolation level | Dirty Read | Nonrepeatable Read | Phantom Read |
|:--:|:--:|:--:|:--:|
|READ_UNCOMMITTED|O|O|O|
|READ_COMMITTED|X|O|O|
|REPREATABLE_READ|X|X|O|
|SERIALIZABLE|X|X|X|



---

# 해결책


## Isolation level 높이기

**MySQL에서는 `Isolation level`을 `SERIALIZABLE`로 올리는 방법** 밖에 없는데 이 경우에 **항상 `shared lock`이 걸리므로 현실적으로 사용하기 힘들다.**

## SELECT FOR UPDATE 사용

업데이트 할 레코드를 가져올 때 **SELECT 쿼리 대신 `SELECT FOR UPDATE` 문을 사용하면 `shared lock`**이 걸린다. 

그러면 `Transaction B`가 **읽기를 시도할 때** `Transaction A`가 **`Commit`이나 `Rollback`**되기까지 **기다리게 되므로 문제가 발생하지 않는다.**

## UPDATE 한번에 모든 것을 처리

**SELECT를 통해 상태 값을 읽어온 후 UPDATE 로직을 수행하지 말고**,
**UPDATE의 조건절을 늘려 SELECT 로직 없이, UPDATE 구문 하나에 처리하는 방법**이다.

이렇게 하면 **비즈니스 로직이 Application 코드에서 SQL로 옮겨가기**는 하지만 **마지막으로 Commit된 Data를 기준으로 작동해서 문제가 발생하지 않는다.**

## 낙관적(optimistic) 락

**Table에 버전 필드를 추가**해서 SELECT할 때 가져온다. 

그리고 **UPDATE할 때 WHERE 절에 기존 버전을 추가**하고 **+1된 버전으로 업데이트를 시도**한다. 

**업데이트 된 레코드 수를 검사해서 0개**라면 **다른 Transaction에서 버전이 변경된 것을 알 수 있다. **

**이렇게 충돌을 감지한 경우** **Application 단에서 전체 Transaction을 처음부터 재시도**해야 할 수도 있다.

**ORM에서 낙관적 락 기능을 제공하는 경우도 있다.**
