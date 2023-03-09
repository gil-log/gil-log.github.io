---
title: "[Error] Spring Boot - The following method did not exist: org.springframework.plugin.core.PluginRegistry. 해결 - Swaager2 관련 에러"
last_modified_at: 2021-06-14T23:20
categories: 
  - error
tags: 
  - 'Springboot' 
  - 'error' 
  - 'swagger2'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[The following method did not exist: 'org.springframework.plugin.core.PluginRegistry[StackOverflow]](https://stackoverflow.com/questions/60710084/the-following-method-did-not-exist-org-springframework-plugin-core-pluginregis)

[]()


---

# 문제 상황

**`Swagger2`를 Spring Boot Project에 적용 하던 중**,

**아래와 같은 오류**와 함께 프로젝트가 시작되지 않았다.

``` bash

***************************
APPLICATION FAILED TO START
***************************

Description:

An attempt was made to call a method that does not exist. The attempt was made from the following location:

    org.springframework.hateoas.server.core.DelegatingLinkRelationProvider.<init>(DelegatingLinkRelationProvider.java:36)

The following method did not exist:

    org.springframework.plugin.core.PluginRegistry.of([Lorg/springframework/plugin/core/Plugin;)Lorg/springframework/plugin/core/PluginRegistry;

The method's class, org.springframework.plugin.core.PluginRegistry, is available from the following locations:

    jar:file:/Users/gillog/.m2/repository/org/springframework/plugin/spring-plugin-core/1.2.0.RELEASE/spring-plugin-core-1.2.0.RELEASE.jar!/org/springframework/plugin/core/PluginRegistry.class

The class hierarchy was loaded from the following locations:

    org.springframework.plugin.core.PluginRegistry: file:/Users/gillog/.m2/repository/org/springframework/plugin/spring-plugin-core/1.2.0.RELEASE/spring-plugin-core-1.2.0.RELEASE.jar
2

Action:

```

---

# 핵심 문구

나는 해당 에러의 핵심 문구로 

```
The following method did not exist:

    org.springframework.plugin.core.PluginRegistry.of
```

이 부분을 정하고, 구글 신의 도움을 청했다.

---

# 해결

**문제의 원인**은 **사용하려던 spring.fox 2.9.2 version**에는 **`org.springframework.plugin.core.Plugin`이 removed 되어 발생한 오류**였다.

![](https://images.velog.io/images/gillog/post/a9b133b5-0470-4561-8747-3a46b78f4765/image.png)

하여 해당 버전을 snapshot으로 변경하였다.

`pom.xml`에 `dependency`와 `repository`를 추가해주었다.
```
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger2</artifactId>
  <version>3.0.0-SNAPSHOT</version>
</dependency>
```


![](https://images.velog.io/images/gillog/post/690c09b6-fac3-46ae-8894-8c9af1f891b9/image.png)

```
<repositories>
  <repository>
    <id>jcenter-snapshots</id>
    <name>jcenter</name>
    <url>http://oss.jfrog.org/artifactory/oss-snapshot-local/</url>
  </repository>
</repositories>
```


![](https://images.velog.io/images/gillog/post/e12e25e0-58fe-4390-8f1f-e773e2a31e2f/image.png)


하여 문제 없이 잘 실행되었다!


![](https://images.velog.io/images/gillog/post/772580fe-31df-40a4-8fc8-f5b97363cff5/image.png)

오늘도 해결 :)  🌟
