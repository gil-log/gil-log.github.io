---
title: "[JPA] @MappedSuperclass로 공통 Entity 상속 받기(Created_At, Modified_At 등 공통 테이블 컬럼)"
last_modified_at: 2021-07-18T12:36
categories: 
  - jpa
tags: 
  - '@MappedSuperclass' 
  - 'JPA' 
  - '공통 Entity'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 ORM 표준 JPA 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)


---

**만약 모든 테이블에 `CREATED_TIME`, `MODIFIED_TIME`과 같은 공통 Column이 있다**고 하자.

**이럴때 `@MappedSuperclass`로 공통 `Entity`를 선언하고 상속받아 관리**할 수 있다.

**아래 코드를 보면 손쉽게 이해되고 사용**할 수 있다.

```java
@MappedSuperclass
public class CommonEntity {
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "CREATED_DATE")
    private Date createdDate;
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "MODIFIED_DATE")
    private Date modifiedDate;
}

@Table(name = "TB_USER")
@Entity
public class User extends CommonEntity {

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

**두 개 이상의 Table에서 사용하는 Column들의 집합**이 있다면,

**일일이 추가하지말고 위와 같이 `@MappedSuperclass`를 활용**해보자.