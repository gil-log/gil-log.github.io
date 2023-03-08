---
title: "Spring vs Spring Boot"
last_modified_at: 2021-07-06T04:29
categories: 
  - spring
tags: 
  - 'AutoConfiguration' 
  - 'Spring' 
  - 'Spring boot' 
  - 'spring-boot-starter'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️


[Spring vs. Spring Boot: A Comparison of These Java Frameworks[DZone-JavaZone]](https://dzone.com/articles/spring-vs-spring-boot)

[[Spring] Spring 과 Spring Boot[Nathan Kwon]](https://monkey3199.github.io/develop/spring/2019/04/14/Spring-And-SpringBoot.html)

<br>

# Spring

**`Spring`은 Java 진영에서 가장 대중적인 동적 웹 개발 프레임워크**이다. 

**`Spring`은 아래 세 가지**
**`의존성 주입(DI, Dependency Injection)`,
`제어의 역전(IOC, Inversion Of Control)`,
`관점 지향 프로그래밍(AOP, Aspect-Oriented Programming)`**


**특성을 이용**해 **결합도를 낮추는 방식으로 Application을 개발**할 수 있다. 


# Spring Boot

**`Spring`의 환경 설정은 복잡**하다.


** `Transaction Manager`, `Hibernate Datasource`, `Entity Manager`, `Session Factory` ...**
 
**위 같은 설정에 초기 프로젝트 구축시 많은 어려움**이 있다. 


**`Spring Boot`는 최소한의 기능으로 `Spring MVC`를 사용하여 손 쉽게 사용하기 위해 등장**했다.


>  Spring Boot makes it easy to create stand-alone, 
production-grade Spring based Applications that you can **"just run".**
https://spring.io/projects/spring-boot


**`spring-boot-starter` Dependency만 추가해주면 바로 API를 정의**하고, 

**내장 Tomcat으로 Web Application Server를 실행**할 수 있다. 

또한 **스프링 홈페이지의 `Spring Initializr.`를 사용하면 바로 실행 가능한 프로젝트를 생성**해준다. 


**실행환경이나 의존성 관리 등의 인프라 관련 등**은 신경쓸 필요 없이 **바로 개발** 할 수 있게 해주는 것이 **`Spring Boot`의 가장 큰 장점**이다.


<br>




**Spring Boot는 손쉬운 환경 설정을 위해 아래와 같은 방법을 사용**했다.



## AutoConfiguration

**`Spring Boot`는 공통적으로 필요한 `DispatcherServlet`같은 설정**을 **`Annotation`을 이용하여 자동으로 설정**해준다.

**`Spring Boot`의 main method**는 **`@SpringBootApplication` Annotation**을 가지고 있다.

```java
@SpringBootApplication
public class BootApplication {
	public static void main(String[] args) {
		SpringApplication.run(BootApplication.class, args);
	}
}
```
_**@SpringBootApplication = @ComponentScan + @configuraion + @EnableAutoConfiguration**_


<br>

**해당 `@SpringBootApplication` 하위에 있는 `Annotation` 들을 모두 `Scanning`하여 자동으로 설정**해준다.

<br>

또한 **`Class path`에 있는 `jar`를 감지하여 자동으로 구성**해준다.


**`Spring`의 `jar`파일이 `Class path`에 있는 경우 `Spring Boot`는 `Dispatcher Servlet`으로 자동 구성**한다. 

마찬가지 **`Hibernate`의 `jar`파일이 `Class path` 내에 존재한다면 이를 `DataSource`로 자동설정**해준다. 



## Spring Boot Starter

**`Spring Boot`는 `spring-boot-starter-web`**를 통해 **개발에 필요한 종속Dependency를 관리**한다. 

**지금까지 Web Application을 위해 개발자들**은 **사용하려는 jar, 사용할 jar 버전등을 명시하며 골머리**를 앓아왔다.

**각각의 jar들의 호환되는 버전들을 찾아가며 일일히 설정해야 했기 때문**이다.

_**Spring MVC, Jackson Databind, Hibernate, Log4j, ...**_

**`Spring Boot`와 함께라면 개발자는 그저 Application을 실행**만 하면 된다. 



- Spring
```
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>4.2.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.5.3</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.0.2.Final</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```




- Spring Boot
```

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```




