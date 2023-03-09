---
title: "[SpringBoot] Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured. 에러 해결"
last_modified_at: 2021-06-03T22:36
categories: 
  - error
tags: 
  - 'Springboot' 
  - 'vscode'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Spring Boot 최초 실행 시 'Failed to configure a DataSource...[From Novice To Guru]](https://fntg.tistory.com/193)

[]()

<br>


**Spring Boot Server를 구동하던 중 아래와 같은 에러가 발생**했다.
![](https://images.velog.io/images/gillog/post/08140b12-522e-4007-b746-dfe357381bb3/image.png)


```bash
Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2021-06-04 07:27:05.008 ERROR 2027 --- [  restartedMain] o.s.b.d.LoggingFailureAnalysisReporter   : 

***************************
APPLICATION FAILED TO START
***************************

Description:

Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class


Action:

Consider the following:
        If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
        If you have database settings to be loaded from a particular profile you may need to activate it (no profiles are currently active).
```

# 핵심 문구

먼저 나는 핵심 문구를 아래로 보고 Google에 검색을 진행해봤다.

대충 DataBase 연동 관련 문제인 것 같은데,,
```
Failed to configure a DataSource: 'url' attribute is not specified 
and no embedded datasource could be configured.
```

확인해 보니 **`application.properties`에 DataBase 연동 정보가 없어 발생한 에러** 였다.

**대부분 최초 Spring Boot 구동 시 많이 발생하는 에러**인 것 같다.


# 해결 방법

Spring Boot Project에 **`application.porperties`에 아래 예시 양식 대로 DataSource 연동 정보를 입력**해주면 된다.
_나는 MySQL 사용 할 거라 mysql 연동 정보를 입력_

```
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver 
spring.datasource.url=jdbc:mysql://localhost:3306/[DB명]?useSSL=false&useUnicode=true&characterEncoding=utf8&serverTimezone=UTC";
spring.datasource.username=[DB접속ID] 
spring.datasource.password=[DB비번]
```

