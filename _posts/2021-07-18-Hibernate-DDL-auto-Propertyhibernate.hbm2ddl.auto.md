---
title: "[Hibernate] DDL auto Property(hibernate.hbm2ddl.auto)"
last_modified_at: 2021-07-18T08:01
categories: 
  - jpa
tags: 
  - 'Hibernate' 
  - 'JPA' 
  - 'ddl auto' 
  - 'hibernate.hbm2ddl.auto'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 ORM 표준 JPA 프로그래밍](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=62681446)


---


**JPA를 사용할 때 Hibernate의 DDL 자동 생성 기능을 사용**할 수 있다.



# 속성 종류

아래와 같은 속성 값을 사용할 수 있다.

|옵션|설명|
|:--:|:--:|
|create|기존 테이블을 삭제하고 새로 생성, DROP + CREATE|
|create-drop| Application 종료 시 생성된 DDL을 삭제, DROP + CREATE + DROP|
|update| DB Table과 Entity Mapping 정보를 비교해 변경 사항은 수정.|
|validate| DB Table과 Entity Mapping 정보를 비교해, <br>차이점이 있을 경우 경고와 함께 Application을 실행하지 않음.|
|none| 자동 생성 기능을 사용하지 않을때, hibernate.hbm2ddl.auto 속성 자체를 삭제하지 않고, <br> 유효하지 않은 속성 값을 추가하면 적용되지 않음.<br> none은 유효하지 않은 속성 중 하나.|

# 속성 설정 방법

**`Hibernate`에서 지원하는 DDL 자동 생성 기능은 아래와 같은 속성을 사용하면 가능**하다.

```xml
// xml 방식
<property name="hibernate.hbm2ddl.auto" value="create">

// application.properties 추가 방식
spring.jpa.hibernate.ddl-auto=create
```
