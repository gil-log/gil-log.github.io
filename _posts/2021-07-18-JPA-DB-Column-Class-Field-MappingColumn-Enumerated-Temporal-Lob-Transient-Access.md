---
title: "[JPA] DB Column, Class Field Mapping(@Column, @Enumerated, @Temporal, @Lob, @Transient, @Access)"
last_modified_at: 2021-07-18T10:25
categories: 
  - jpa
tags: 
  - '@Temporal' 
  - '@lob' 
  - 'COLUMN' 
  - 'Column Field Mapping' 
  - 'JPA' 
  - 'access' 
  - 'enumerated' 
  - 'transient'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 ORM 표준 JPA 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)


---


`JPA`에서 **`@Entity`로 `Table`을 매핑하는 Class의 `Field` 값**과,

**`Table`의 `Column` Mapping에 사용되는 Annotation**으로,

**`@Column`, `@Enumerated`, `@Temporal`, `@Lob`, `@Transient`, `@Access`** 가 있다.

**아래는 해당 Annotation들의 설명**이다.



# @Column

**`@Column`은 객체 Field를 Table Column과 Mapping**해준다.

[@Column에 대한 설명은 해당 글을 통해서 확인할 수 있다.](https://velog.io/@gillog/JPA-Column-Annotation)

# @Enumerated
  
**Java의 `enum` Type과 Mapping할때 사용**된다.

**사용되는 속성은 아래 `value` 뿐**인데, **`value`로 지정되는 값들에 대한 설명은 아래**와 같다.

|값|설명|
|:--:|:--:|
|EnumType.ORDINAL|해당 enum의 순서 값인 숫자를 DB에 저장|
|EnumType.STRING|해당 enum의 이름을 DB에 저장|

이 중 **기본 값은 `EnumType.ORDINAL`**인데, 

**`EnumType.ORDINAL`의 장점은 저장되는 크기가 작다는 장점뿐**인데,

**저장된 enum의 순서가 변경되면 굉장히 큰 혼란을 야기**할 수 있다.

거의 **필수적으로 `EnumType.STRING`으로 지정**해주자.

아래는 사용 예시이다.

```java
@Table(name = "TB_LICENSE")
@Entity
public class License {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "L_ID")
    private long id;

    @Enumerated(value = EnumType.STRING)
    @Column(name = "type")
    private LicenseType type;

}
```

# @Temporal

**날짜 Type(`java.util.Date`, `java.util.Calendar`) Mapping에 사용**된다.

**사용되는 속성은 `value`로 지정 값들에 대한 설명은 아래**와 같다.

|값|설명|
|:--:|:--:|
|TemporalType.DATE|날짜, DB `date` Type과 매핑(2021-07-18)|
|TemporalType.TIME|시간, DB `time` Type과 매핑(19:23:15)|
|TemporalType.TIMESTAMP|날짜와 시간, DB `timestamp` Type과 매핑(2021-07-18 19:23:15)|

아래는 사용 예시이다.

```java
@Getter
@MappedSuperclass
public class CommonEntity {
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "CREATED_DATE")
    private Date createdDate;
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "MODIFIED_DATE")
    private Date modifiedDate;
}
```

# @Lob

**DB `CLOB`, `BLOB` Type과 Mapping 하는데 사용**한다.

`@Lob` Annotation은 **지정 가능한 속성이 없고**,

Mapping **Field Type이 문자 형태면 `CLOB`과 Mapping** 하고,

**나머지 형태들은 모두 `BLOB`과 Mapping**한다.

아래는 **`CLOB`, `BLOB`과 Mapping 되는 Java Field Type들**이다.

- **CLOB : `String`, `char[]`, `java.sql.CLOB`**
- **BLOB : `byte[]`, `java.sql.BLOB`**


아래는 **Java에서 Field Type 별, DBMS에 Mapping되는 Column Type의 예시**이다.

```java
@Lob
private String clobField;

@Lob
private byte[] blobField;

// DB Type
// MySQL
clobField longtext
, blobField longblob

// Oracle

clobField clob
, blobField blob

// PostgreSQL
clobField text
, blobField old
```

# @Transient

**`@Transient`로 지정된 Field는 Mapping되지 않는다.**

**DB에 저장하지 않고 조회도 하지 않는다.**

**단지 Java 객체에 값을 보관할고 싶을 경우에 사용**한다.


```java
@Transient
private List<T> tempList;
```

# @Access

**`@Access` Annotation**은 **`Entity Class`에 지정**되어 **`JPA`가 해당 `Entity` Data에 접근하는 방식을 지정** 한다.


**접근 방식은 `AccessType.FIELD`와 `AccessType.PROPERTY` 두 가지**이다.

- **AccessType.FIELD** : **Field가 private로 설정되어도 Field에 직접 접근**한다.
- **AccessType.PROPERTY** : **접근자(Getter)를 통해 접근**한다.

```java
@Access(AccessType.FIELD)
@Entity
public class User {
    @Id
    private long id;
    
    private String name;
    
    public long getId() {
    	return id;
    }
    
    public String getName() {
    	return name;
    }
}
```

만약 **`@Access`를 설정하지 않을 경우**, **`@Id` 위치를 기준으로 접근 방식을 설정**한다.

## @Id 위치 = Field

**`@Acess(AccessType.FIELD)` 설정처럼 Field에 직접 접근**한다.

```java
@Entity
public class User {
    @Id
    private long id;
    
    private String name;
    
    public long getId() {
    	return id;
    }
    
    public String getName() {
    	return name;
    }
}
```

## @Id 위치 = Property


**`@Acess(AccessType.PROPERTY)` 설정처럼 접근자 Getter를 통해 접근**한다.

```java
@Entity
public class User {
    
    private long id;
    
    private String name;
    
    @Id
    public long getId() {
    	return id;
    }
    
    public String getName() {
    	return name;
    }
}
```

## Field, Property 접근 방식 혼용

**`@Id`를 Field에 두고 기본 Data 접근 방식을 `Field`로 설정**하고,

**특정 Field에 `@Access(AccessType.PROPERTY)`**를 통해 ** 특정 Field만 접근자 Getter 방식으로 접근**할 수 있다.

```java
@Entity
public class User {
    
    @Id
    private long id;
    
    @Transient
    private String name;
    
    private String loginId;
    
    public long getId() {
    	return id;
    }
    
    @Access(AccessType.PROPERTY)
    public String getLoginId() {
    	return name + "gillog";
    }
}
```

위 사용 예제는 **다른 Field들은 `@Id`가 Field에 존재**해 **AccessType.FIELD로 접근**하고,

**`getter`에 `@Access(AccessType.PROPERTY)`가 설정된 `loginId`만 AccessType.PROPERTY로 접근**한다.


**`name` `Field`는 DB와 저장, 조회 되지 않고**,
_@Transient 사용_

**`loginId` Column에는 `name` Field 값에 `gillog`가 붙은 값이 저장**된다.