---
title: "[JPA] 기본 키 생성 전략(IDENTITY, SEQUENCE, TABLE)"
last_modified_at: 2021-07-18T09:22
categories: 
  - jpa
tags: 
  - 'IDENTITY' 
  - 'JPA' 
  - 'Table' 
  - 'sequence'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 ORM 표준 JPA 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)


---

# 기본 키 할당 전략

**JPA가 제공하는 DB 기본 키 할당 전략**은 **`직접 할당 방식`, `자동 생성 방식` 두 가지**이다.

이 중 **`직접 할당 방식`은 Application에서 기본 키를 직접 할당하는 방식**이다.

**`자동 생성 방식`은 대리 키를 사용하는 방식**으로 **`IDENTITY`, `SEQUENCE`, `TABLE`** **네 가지가 있다.**

**해당 방식들은 사용하는 DB에 의존**한다.
_`MySQL`은 `IDENTITY` 사용, `Oracle`은 `SEQUENCE` 사용_


---

# 직접 할당 방식


**직접 할당 방식을 사용할 경우** **Entity를 생성할 때 Key Column에 `@Id`만 사용**해 주어도 된다.

```java
@Id
private long id;
```

**`@Id`가 적용 가능한 Java Type은 아래**와 같다.

- Java 기본형(int, double, long ...)
- Java Wrapper 형
- String
- java.util.Date
- java.sql.Date
- java.math.BigDecimal
- java.math.BigInteger

**해당 전략**은 **`em.persist()`로 Entity를 저장 하기 전**,

**Application에서 직접 기본 키를 할당해주어야 한다.**

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("JPA");

EntityManager em = emf.createEntityManager();

User user = new User();
user.setName("gillog");
user.setId(1);

em.persist(user);
```


# 자동 생성 방식


**`자동 생성 방식`을 사용할 경우 `@Id`와 `@GeneratedValue`를 사용**한다.


## IDENTITY

**기본 키 생성을 DB에 위임하는 전략**이다.

**`MySQL`과 같이 Sequence를 제공하지 않고, `AutoIncrement` 기능을 제공**해,

**기본 키 값을 자동으로 생성하는 DBMS에서 사용**한다.

**주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용**한다.

아래와 같이 사용할 수있다.

```java
@Id
@GeneratedValue=strategy = GenerationType.IDENTITY)
private long id;
```

**`IDENTITY`전략은 Data를 DB에 Insert한 후 기본 키 값을 조회**할 수 있다.

**Entity에 식별자 값을 할당하려면 JPA는 추가로 DB를 조회해야** 하는데,

**`Hibernate`는 `JDBC3`에 추가된 `Statement.getGeneratedKeys()`를 사용**해 **DB와 한번만 통신**한다.
_해당 메소드는 Data를 저장하면서, 생성된 기본 키 값을 가져온다._


<br>

하지만 **Entity가 영속 상태가 되려면 식별자가 반드시 필요**한데,

**DB에 저장해야만 식별자를 구할 수 있는 방식**이므로,

**`em.persist()`호출 즉시 Insert Query를 DB에 전달해 식별자를** 가져오므로,

**Transaction을 지원하는 쓰기 지연 방식이 동작하지 않는다.**


## SEQUENCE

**해당 방식**은 **DB Sequence를 사용해 기본 키를 할당하는 방식**으로,

**Sequence 전략을 지원하는 Oracle, PostgreSQL, DB2, H2 DB에서 주로 사용**한다.

아래와 같이 **DB에서 Seqeunce를 생성 후에 사용**할 수 있다.

```sql
CREATE SEQUENCE USER_SEQ START WITH 1 INCREMENT BY 1;
```

**후에 Entity를 생성할 때 `@SequenceGenerator` Annotation을 사용**한다.

```java
@Entity
@SequenceGenerator(
	name = "USER_SEQ_GENERATOR"
    , sequenceName = "USER_SEQ"
    , initialValue = 1
    , allocationSize = 1
)
public class User {

    @Id
    @GeneratedValue(
    	strategy = GenerationType.SEQUENCE
    	, generator = "USER_SEQ_GENERATOR"
    )
    private long id;
    
}
```

**`@SequenceGenerator` Annotation에서 사용 가능한 속성들은 아래**와 같다.

|속성|설명|기본 값|
|:--:|:--:|:--:|
|name|식별자 생성기 이름|없음 지정 필수.|
|sequenceName|DB에 등록되어 있는 Sequence이름|hibernate_sequence|
|initalValue(DDL)|DDL 생성시에만 사용, Sequence DDL 생성시 처음 시작 value를 설정|1|
|allocationSize|Sequence 한번 호출시 증가하는 수(성능 최적화에 사용)|50|
|catalog, schema|DB catalog, schema 이름||

### allocationSize와 성능 최적화

**`allocationSize`가 기본값이 `50`이므로 해당 속성을 `1`로 설정하지 않을 시**,

**sequence 호출 시마다 50씩 증가**한다.

**기본 값이 `50`인 이유**는 **JPA가 sequence에 접근 횟수를 줄이기 위함**이다.


**`allocationSize`에 설정 값 만큼 한 번에 sequence를 증가** 시키고, **그만큼 `Memory`에 seqeunce 값을 할당**한다.

그 후 **`Memory`를 활용해 `JVM`안에서 sequence를 할당** 한다.

**이 방법은 sequenc를 선점**하여 다른 **여러 `JVM`이 동시 동작해도 기본 키 값이 충돌하지 않는 장점**이 있다.



<br>

### IDENTITY VS SEQUENCE

**`IDENTITY` 전략**은 **먼저 Entity를 DB에 저장한 후**에,

**식별자를 조회해 Entity의 식별자로 할당하는 전략**이다.

<br>

**`SEQUENCE` 전략**은 **`em.persist()` 호출 전에 먼저 DB Sequence를 먼저 조회**한다.

그 후 **조회한 식별자를 Entity에 할당한 후 Entity를 영속상태로 저장**한다.

그 후 **`Transaction`을 `Commit`하여 `Flush`가 발생할 때 해당 Entity를 DB에 저장**한다.


## TABLE

**해당 전략**은 **Key 생성 Table을 사용하는 전략**이다.


**Key 생성 전용 Table을 생성**하고, **name, value로 사용할 Column을 생성**하여

**DB Sequence를 흉내내는 전략**이다.


**아래와 같이 Key 생성 전용 Table을 생성**한 후,

```sql
CREATE TABLE CUSTOM_SEQUENCE {
	sequence_name varchar(255) not null
    , next_val bigint
    , primary key (sequence_name)
}
```

**`@TableGenerator` Annotation을 사용해 Entity를 Mapping**한다.

```java
@Entity
@TableGenerator(
	name = "USER_SEQ_GENERATOR"
    , table = "CUSTOM_SEQUENCE"
    , pkColumnValue = "USER_SEQ"
    , allocationSize = 1
)
public class User {

    @Id
    @GeneratedValue(
    	strategy = GenerationType.TABLE
    	, generator = "USER_SEQ_GENERATOR"
    )
    private long id;
    
}
```

**해당 전략은 `SEQUENCE` 전략과 내부 동작 방식이 같다.**

**`pkColumnValue`로 설정한 `USER_SEQ`라는 SEQUENCE NAME**을 **`CUSTOM_SEQUENCE` Table의 `sequence_name` Column에 생성**한 후,
**`next_val` Column의 값이 증가**한다.
_값이 없으면 JPA가 Insert하면서 초기화하므로 미리 값을 넣어두지 않아도 됨_


**`@TableGenerator`에서 사용 가능한 속성들은 아래**와 같다.

|속성|설명|기본 값|
|:--:|:--:|:--:|
|name|식별자 생성기 이름|없음 지정 필수.|
|table|키 생성 테이블 명|hibernate_sequences|
|pkColumnName|시퀀스 컬럼 명|sequence_name|
|valueColumnName|시퀀스 값 컬럼 명|next_val|
|pkColumnValue|키로 사용할 값 이름|Entity 이름|
|initialValue|초기 값, 마지막 생성된 값이 기준|0|
|allocationSzie|시퀀스 호출시 증가 값(성능 최적화에 사용)|50|
|catalog, schema|DB catalog, schema 이름||
|uniqueConstraints(DDL)|유니크 제약 조건을 지정||


**`TABLE` 전략은 값을 조회하면서 Select Query**를, 그 후 **값 증가를 위해 Update Query를 사용**한다.

**`SEQUENCE` 전략과 비교해 DB와 한번 더 통신하는 단점**이 있고,

**`SEQUENCE` 전략의 최적화와 같이 `allocationSize`를 사용해 최적화** 한다.

## AUTO

**`AUTO` 전략**은 **선택한 DB 방언에 따라 자동으로 `IDENTITY`, `SEQUENCE`, `TABLE` 전략을 자동으로 선택**한다.
_DB의 종류도 많고, 기본 키 생성 방식도 다양하기에_

**해당 전략은 DB를 변경해도 Code 수정을 하지 않아도 되는 장점**과,

**Key 생성 전략이 정해지지 않은 개발 초기 단계**나 **프로토타입 개발 시에 편리하게 사용**할 수있다.

하지만 **만약 `AUTO`로 `SEQUENCE`나 `TABLE`전략이 선택 될 경우**,

**Sequence나 키 생성 Table을 미리 생성**해 두어야 한다.

만약 **DDL 자동 생성 기능을 사용한다면**, **`Hibernate`가 기본 값을 사용해 알아서 생성**해준다.