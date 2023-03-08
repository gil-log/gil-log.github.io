---
title: "[Spring] profile로 서버 환경에 맞는 Context 적용하기(-Dspring.profiles.active)"
last_modified_at: 2021-07-07T03:06
categories: 
  - spring
tags: 
  - '-Dspring.profiles.active' 
  - 'Spring' 
  - 'profile' 
  - 'tomcat'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[docs.spring.io[Profile Annotation]](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Profile.html)


<br>

**복수 이상의 WAS를 사용하는 상황**에서

**같은 Spring Application을 사용**하지만 **각 Sever에 맞게 의 Context를 설정해야 하는 경우**가 있다.


**예를 들자면 아래와 같은 상황**이다.

> **1.23.45.66 Tomcat Server**에서는 **DB 접속을 3.33.33.33:3306에 접속**해야 한다.
메인 Server에 접속하기 위한 **Token은 "abcdefghgillog11"** 이다.
**2.44.51.77 Tomcat Server**에서는 **DB 접속을 4.44.44.44:3306에 접속**해야 한다.
메인 Server에 접속하기 위한 **Token은 "gillogggolillog124"** 이다.

**위와 같은 상황에서 활용될 수 있는것이 `Spring.Profile`**이다.

**먼저 Spring Profile에 대해 간략히** 알아보자.

# Spring Profile

**`profile`은 세 가지 방법으로 활성화 할 수 있는 논리적 그룹**이다.

- **`ConfigurableEnvironment.setActiveProfiles(java.lang.String...)`**(**Application Code**)
- **`spring.profiles.active` Property** 지정 (**JVM 시스템 속성,환경 변수**)
- **`web.xml`**(tomcat)에서 **Servlet context Parameter**


## @Profile 사용처

**`@Profile`은 아래 세 가지 방법으로 활용 가능**하다.

- **`@Configuration` 포함 모든 `@Component` Class Annotation과 함께 사용**

- **`Custom Stereo Type Annotation` 생성을 위한 `Meta Annotation`으로 활용**

- **`@Bean` Annotation이 사용된 method와 함께 활용**

## profile 문자열 활용

먼저 **profile 관련 표현식은 아래**와 같다.

- `!` NOT
- `&` AND
- `|` OR

**profile은 아래와 같이 활용 가능**하다.


```java
// server1이나 server2에서 활성화
@Profile({"server1", "server2"})

// server1이거나 server2가 아닐 경우 활성화
@Profile({"server1", "!server2"})

// server1이고 server2가 아닌 경우 이거나 아예 server3인 경우
@Profile("(server1 & !server2) | server3")
```

이는 **`Spring Context.xml` 등에서 `<beans profile="p1,p2">`과 같이 설정하는 것과 같다.**



# Profile Tomcat 활용

**`spring.profiles.active` Property 지정 (JVM 시스템 속성,환경 변수) 방법**을 통해서,

**Tomcat 각 Server 별 `Profile`을 지정**해보려한다.

**먼저 `Tomcat`의 `launch configuration`에** 들어가자


![](https://images.velog.io/images/gillog/post/091ad566-5ff2-4151-839f-bf3817e258e4/image.png)


이제 **`Arguments`에 `VM arguments`로 구동시 변수 설정**으로 ,
**`-Dspring.profiles.active=local`** 와 같이 **해당 Tomcat Server에서 설정하고 싶은 `Profile 명` 을 설정**하자.

![](https://images.velog.io/images/gillog/post/e7b2b140-f1ce-47c0-a251-037e1a6b6c08/image.png)


이제 **Java안에서 `@Configuration`이나 `@Bean` 관련 Annotation에서 `@Profile`을 통해 활용**하거나,

**`context관련.xml` 에서 아래와 같이 활용 가능**하다.

```xml
<beans profile="local">
  <bean id="datasource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
    <property name="url" value="jdbc:mysql://3.33.33.33:3306/gillog">
  </bean>
  <bean id="token">
    <property name="jwt" value="abcdefghgillog11">
  </bean>
</beans>
    
    
<beans profile="devel">
  <bean id="datasource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
    <property name="url" value="jdbc:mysql://4.44.44.44:3306/gillog">
  </bean>
  <bean id="token">
    <property name="jwt" value="gillogggolillog124">
  </bean>
</beans>
```

이를 통해 **`spring.profile.active=local`로 지정한 Tomcat Server**와,

**`spring.profile.active=devel`로 지정한 Tomcat Server**에서,

**각각 다른 `Bean`으로 같은 Spring Application을 구동 가능**하다.