---
title: "Transaction(ACID, Commit, Rollback)"
last_modified_at: 2021-05-22T09:08
categories: 
  - 개념
tags: 
  - 'ACID' 
  - 'Atomicity' 
  - 'Isolation' 
  - 'consistency' 
  - 'durability' 
  - 'transaction'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[docs.spring.io[transaction-declarative-annotations]](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative-annotations)

[Transaction[개발자 이준스]](https://juns-lee.tistory.com/m/entry/Transaction?category=830260)

[Spring Transaction 관리[Naming Tom]](https://naming0617.tistory.com/32)


---

# Transaction

**`Transaction`이란, 더 이상 나눌 수 없는 작업 단위(unit of work)**을 말한다.

다른 의미로는 **DBMS에서 상호작용의 단위**, **DB의 상태를 변환 시키는 하나의 논리적 기능 수행을 위한 작업 단위**, **한번에 모두 수행되어야 하는 일련의 연산을 의미**한다.


**`Transaction`은 안전한 수행을 보장하기 위해 `ACID` 네 가지 성질**을 가지고 있다.



## ACID

**`ACID`**는 **`Atomicity(원자성)`, `Consistency(일관성)`, `Isolation(고립성)`, `Durability(지속성)` 네 가지 성질을 의미**한다.

### Atomicity(원자성)

**`Atomicity(원자성)`**은 **`Transaction`과 관련 작업들이 부분적으로 실행되다 중단되지 않는 것을 보장하는 성질**이다.

>[EX] Gil이 Log에게 송금하기 위해 **계좌에서 돈이 빠져나갔는데, Log의 계좌에 돈이 입금이 안되고** **Transaction이 종료되는 상황은 일어나선 안된다.**

### Consistency(일관성)

**`Consistency(일관성)`**은 **`Transaction`이 성공적으로 수행되면 언제나 일관성 있는 DB 상태, 여러 제약 조건에 맞는 상태로 유지하는 성질**이다.


### Isolation(고립성)

**`Isolation(고립성)`**은 **`Transaction` 수행 시 다른 `Transaction` 연산 작업이 끼어들지 못하도록 보장하는 성질**이다.

>[EX] 송금을 위해 **돈이 빠져나간 Gil의 잔고와 아직 잔고가 늘지 않은 Log의 계좌**의 **DB 상황, 즉 완료되지 않은 Transaction 상황을 다른 Transaction에서 read하면 안된다.**


### Durability(지속성)


**`Durability(지속성)`**은 **성공적으로 수행된 `Transaction`은 지속적으로 영원하게 반영되어야 하는 것을 의미**한다.

>[EX] Gil에서 Log의 계좌로 **송금 작업이 끝난 Transaction은 그 후에 장애가 발생**하더라도 **성공 상태로 복구가 가능**해야한다.

---

## Transaction Rollback

DB 엔진 자체에서 **하나의 쿼리에 대해서는 완벽한 `Transaction`을 지원**한다.

하지만 **여러 가지 쿼리가 수행되는 작업**을 **하나의 `Transaction`으로 취급해야 하는 경우에는 얘기가 달라진다.**

**여러 쿼리가 하나의 `Transaction`**이 되려면, **중간에 어떤 쿼리의 수행이 실패**할 경우 **앞서 성공적으로 실행된 쿼리의 수행 작업도 취소되어야 한다.**

**이러한 취소 작업을 `Transaction Rollback`**이라고 한다.


## Transaction Commit

**`Transaction Commit`**은 **여러개의 쿼리의 수행 작업이 실행되고 모든 쿼리의 수행이 성공적으로 종료**되면 **해당 `Transaction`작업을 확정시키기 위해 DBMS에 알려주는 것**이다.

---

## Using Transaction

**`Transaction`이 사용되기에 적절한 경우**는 **결제 시스템, 예약 시스템 처럼 DB에서 여러 테이블에 걸쳐 여러 입/출력 쿼리가 수행되는 시스템에서 사용**된다.


### In JDBC

```java
public void payment(Account account) throws Exception {
    Connection conn = dataSource.getConnection();  
    conn.setAutoCommit(false);

    try {
        paymentDao.setAccountPrice(account);
        paymentDao.setPayment(account.getPaymentType());

        conn.commit();
    }
    catch(Exception e) {
        conn.rollback();
    }
    conn.close();
}
```

위 코드는 **`JDBC` 에서 계정의 결제 금액을 처리하는 `setAccountPrice`**와 **결제에 대한 정보를 저장하는`setPayment`**를 **하나의  `Transaction`으로 작업하기 위한 코드**이다. 

**결제 금액 처리와, 결제 정보 저장을 하나의 작업 단위**로 묶고 **어느 한 지점에서 에러가 발생하면 RollBack이 수행되도록 처리**되었다.

### Limit JDBC Transaction

**`JDBC`는 Data Access에 독립적이지 않다.**

**`JDBC Transaction`**은 **`Local Transaction`으로 두 개 이상의 DB에 대한 작업을 하나의 `Transaction`으로 만드는 것이 불가능**하다.

**여러 DB에 Data를 넣는 작업을 수행해야 하는 경우 `JTA`를 활용**해야 하지만,
**이럴 때는 `JDBC Transaction` 설정 코드 모두를 변경**해야 하여,
**Service Tier의 비즈니스 로직은 변경되지 않았음**에도, **기술 스택의 변경으로 인해 코드가 바뀌어야 하는 상황이 발생**한다.

---

### In Spring

**Spring Framwork**에서는 **`PlatformTransactionManager` 추상화를 통해서 여러 DB 환경**에서도 **연속적인 Programming Model 구현이 가능하도록 설계**되었다.

![](https://images.velog.io/images/gillog/post/dbad2041-d5b9-4203-8fd3-e94f67c65b3f/image.png)

**`PlatformTransactionManger` Interface**를 **Spring config에 Bean 등록** 한 후 **DI를 받아 사용**하면 **`JDBC`로 작성한 `Transaction` Code를 아래 처럼 수정**할 수 있다.

```
<bean id="paymentService" class="spring.test.service.PaymentService">
    <property name="paymentDao" ref="paymentDao" />
    <property name="transactionManager" ref="transactionManager" />
</bean>

<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionMager">
    <property name="dataSource" ref="dataSource" />
</bean>
```

```java
private PlatformTransactionManager transactionManager;

public void setTransactionManager(PlatformTransactionManager transactionManager){
    this.transactionManager = transactionManager;
}

public payment(Account account) throws Exception {
    TransactionStatus status =
    	this.transactionManager.getTransaction(new DefaultTransactionDefinition());

    try {
        paymentDao.setAccountPrice(account);
        paymentDao.setPayment(account.getPaymentType());

        this.transactionManager.commit(status);
    }
    catch(Exception e) {
        this.transactionManager.rollback(status);
        throw e;
    }
```

**Bean으로 등록된 `PlatformTransactionManager`에서 설정된 API를 사용**하여 **Service Tier에서 독립적이고 확장적으로 개발**할 수 있다.

**Data Access 스택이 변경**될 경우 **Spring Config 파일의 설정을 변경함**으로써 **전체적인 시스템에 대한 일괄 적용이 가능**하고, **Service Tier에서는 비즈니스 로직에 대한 코드만 작성**할 수 있다.
