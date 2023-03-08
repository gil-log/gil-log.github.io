---
title: "[JPA] @Column Annotation"
last_modified_at: 2021-07-18T08:16
categories: 
  - jpa
tags: 
  - 'COLUMN' 
  - 'JPA'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 ORM 표준 JPA 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)


---

# @Column

**JPA에서 DB Table의 Column을 Mapping 할 때 `@Column` Annotation을 사용**한다.


```java

@Table(name = "TB_USER")
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="U_ID")
    private long id;
    
    @Column(name="ID")
    private String userId;

    @Column(name="NICK_NAME")
    private String nickName;

    @Builder
    public User(long id, String nickName) {
        this.id = id;
        this.nickName = nickName;
    }
}
```

여기서 **`@Column`을 사용하지 않으면** `userId`와 같이 **속성명 그대로 DB Column과 Mapping을 시도**한다.

**`@Column` Annotation은 아래와 같은 속성 값**이 있다.

|속성|설명|기본값|
|:--:|:--:|:--:|
|name|Mapping할 Column의 이름을 지정.|객체 Field 이름|
|insertable|Entity 저장 시 해당 Field도 저장, <br>false로 읽기 전용 설정 가능.|true|
|updatable|Entity 수정 시 해당 Field도 수정, <br>false로 읽기 전용 설정 가능.|true|
|table|하나의 Entity설정에서 두 개이상 Table에 매핑할 때 사용|현재 Class가 매핑된 Table|
|nullable(DDL)|true/false로 null 허용 여부 설정|true|
|unique(DDL)|true/false로 Unique 제약 조건 설정||
|length(DDL)|Column 속성 길이 설정|255|
|columnDefinition(DDL)|DB Column 정보를 직접 설정|Java Type과 설정 DB 방언으로,<br> 적절한 Column Type 생성|
|precision, scale(DDL)|BigDecimal, BigInteger Type에서 사용, <br> precision은 소수점 포함 전체 자릿수,<Br> scale은 소수 자릿수,<br> double, float Type에는 적용되지 않음.<br>아주 큰 숫자나 정밀한 소수를 다룰때 사용.|precision=19, scale=2|


여기서 **`name`, `insertable`, `updatable`, `table`을 제외한 나머지 속성들은 DDL 생성 기능을 사용할 때만 사용되는 속성**들로,

**JPA 실행 로직에는 영향을 끼치지 않는 속성들**이다.

**직접 DDL을 설정하여 DB Table을 구성할 경우 사용할 이유가 없다.**
_**Entity만으로 개발자가 DB Table 구조 파악이 가능하다는 장점**_


위 속성 중 **`nullable`의 경우 Java의 기본 타입(int, long, ...)은 null 값 입력이 불가능** 하므로,
  
**`false`를 통해 DB Column에 `Not Null` 제약 조건을 지정해 두는것이 안전**하다.
_혹은 직접 DB Column에 Not Null 제약 조건 추가_
  