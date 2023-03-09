---
title: "[JPA] Spring Boot JPA Entity Table 대, 소문자 구분 못하는 경우 해결"
last_modified_at: 2021-07-10T00:24
categories: 
  - error
tags: 
  - 'Hibernate' 
  - 'JPA' 
  - 'Springboot'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Springboot jpa & Hibernate Naming Strategy(네이밍 전략)[IT.FARMER]](https://mycup.tistory.com/237)


---

# 문제 상황

**`SpringBoot`에서 `JPA`**를 통해서 **`Entity` Class**에 **`@Table` Annotation으로 DB Table 명을 아래 사진처럼 대문자로 입력**했는데,

![](https://images.velog.io/images/gillog/post/b6ae3e8a-4219-475c-a1c9-3169108fd44a/image.png)

실제로 **`Hibernate`의 Query 실행 결과를 보니 소문자로 매핑**되고 있었고,

**대문자로 생성된 Table과 매핑이 안되고 있었다.**


![](https://images.velog.io/images/gillog/post/79ff9347-8c4c-4eb5-8c15-e09cfa75a493/image.png)

그래서 **`application.properties`에서 `spring.jpa.hibernate.ddl-auto=create`로 설정**하고,

**DB Schema를 확인해보니 소문자 Table이 생성되어있는걸 확인**했다.

![](https://images.velog.io/images/gillog/post/26ac605f-9d60-4c57-aaf9-16b514b00e1c/image.png)

# 문제 원인

나는 **핵심 문구**로 **`JPA`, `Entity`, `Table`, `UPPER`, `NOTWORKING` 등의 검색어**로 **Google 신께 신탁**했고, **원인을 발견**했다.

해당 **원인은 `Spring Boot`의 `DB Physical Naming Strategy` 때문**이었다.

**`Spring Boot`의 기본 DB Physical Naming 전략은 아래**와 같았다.

> **모든 도트는 밑줄**로 대체, **Camel Case 대문자는 밑줄**로 대체, 모든 **테이블은 소문자**로 구성

# 문제 해결

**`Spring Boot`의 `Physical Naming Strategy`**를,

**`hibernate`의 `Physical Naming Strategy`로 변경하여 해결**했다.

<br>

**`hibernate`의 `PhysicalNamingStrategyStandardImpl` 전략**은,

**설정한 변수 이름을 그대로 사용**하여, 

**`@Table`에서 지정한 설정대로 그대로 사용**할 수 있다.

<br>

**`application.properties`에 아래 설정을 추가**해주면 된다.

```
spring.jpa.hibernate.naming.physical-strategy = org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

![](https://images.velog.io/images/gillog/post/1675bf3e-cddc-4c35-b1c3-a49c962189d3/image.png)


**설정 변경후, 정상적으로 대문자로 Query가 생성되어 정상적으로 매핑**되었다.

![](https://images.velog.io/images/gillog/post/188b0000-144d-4993-82fc-8aa8a2034bff/image.png)


오늘도 해결 완료 :)   🙆🏻‍♂️
