---
title: "[VSCode] jUnit Test 실행 안될 때, Could not detect default configuration classes for test class"
last_modified_at: 2021-06-08T23:48
categories: 
  - error
tags: 
  - 'error' 
  - 'junit' 
  - 'testcode' 
  - 'vscode'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[SpringRunner unable to detect configuration[StackOverflow]](https://stackoverflow.com/questions/52274066/springrunner-unable-to-detect-configuration/52274408)



---

# 문제 상황

**`VSCode`에서 `jUnit`으로 TestCode 작성 중 테스트 실행 자체가 안되는 문제가 발생**했다.


![](https://images.velog.io/images/gillog/post/b8256889-c51e-4680-ac6d-e33181bdf291/image.png)


```
Message:
N/A
Stack trace:
java.lang.Exception: No tests found matching [{ExactMatcher:fDisplayName=saveAndGetUser], {ExactMatcher:fDisplayName=saveAndGetUser(com.ddaja.repository.UserRepositoryTest)], {LeadingIdentifierMatcher:fClassName=com.ddaja.repository.UserRepositoryTest,fLeadingIdentifier=saveAndGetUser]] from org.junit.internal.requests.ClassRequest@491666ad
	at org.junit.internal.requests.FilterRequest.getRunner(FilterRequest.java:40)
	at org.eclipse.jdt.internal.junit4.runner.JUnit4TestLoader.createFilteredTest(JUnit4TestLoader.java:83)
	at org.eclipse.jdt.internal.junit4.runner.JUnit4TestLoader.createTest(JUnit4TestLoader.java:74)
	at org.eclipse.jdt.internal.junit4.runner.JUnit4TestLoader.loadTests(JUnit4TestLoader.java:49)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:513)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:756)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.run(RemoteTestRunner.java:452)
	at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.main(RemoteTestRunner.java:210)
```


```
08:29:34.864 [main] DEBUG org.springframework.test.context.junit4.SpringJUnit4ClassRunner - SpringJUnit4ClassRunner constructor called with [class com.ddaja.repository.UserRepositoryTest]
08:29:34.870 [main] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating CacheAwareContextLoaderDelegate from class [org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate]
08:29:34.886 [main] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating BootstrapContext using constructor [public org.springframework.test.context.support.DefaultBootstrapContext(java.lang.Class,org.springframework.test.context.CacheAwareContextLoaderDelegate)]
08:29:34.963 [main] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating TestContextBootstrapper for test class [com.ddaja.repository.UserRepositoryTest] from class [org.springframework.boot.test.context.SpringBootTestContextBootstrapper]
08:29:34.979 [main] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Neither @ContextConfiguration nor @ContextHierarchy found for test class [com.ddaja.repository.UserRepositoryTest], using SpringBootContextLoader
08:29:34.985 [main] DEBUG org.springframework.test.context.support.AbstractContextLoader - Did not detect default resource location for test class [com.ddaja.repository.UserRepositoryTest]: class path resource [com/ddaja/repository/UserRepositoryTest-context.xml] does not exist
08:29:34.985 [main] DEBUG org.springframework.test.context.support.AbstractContextLoader - Did not detect default resource location for test class [com.ddaja.repository.UserRepositoryTest]: class path resource [com/ddaja/repository/UserRepositoryTestContext.groovy] does not exist
08:29:34.985 [main] INFO org.springframework.test.context.support.AbstractContextLoader - Could not detect default resource locations for test class [com.ddaja.repository.UserRepositoryTest]: no resource found for suffixes {-context.xml, Context.groovy}.
08:29:34.987 [main] INFO org.springframework.test.context.support.AnnotationConfigContextLoaderUtils - Could not detect default configuration classes for test class [com.ddaja.repository.UserRepositoryTest]: UserRepositoryTest does not declare any static, non-private, non-final, nested classes annotated with @Configuration.
08:29:35.133 [main] DEBUG org.springframework.test.context.support.ActiveProfilesUtils - Could not find an 'annotation declaring class' for annotation type [org.springframework.test.context.ActiveProfiles] and class [com.ddaja.repository.UserRepositoryTest]
```

# 핵심 문구

![](https://images.velog.io/images/gillog/post/1ea88489-09cd-4e71-9c7c-b1833de1660e/image.png)

나는 **핵심 문구를 `Did not detct default resource location for test class`**로 정하고,

역시나 구글 신께 여쭈어 보았다.


# 원인

**원인**은 **Spring Boot의 `@SpringBootApplication` Annotation으로 설정한 Class 명**과,
**Test수행을 위해 작성한 Class 명이 일치하지 않아서 발생**했다.

![](https://images.velog.io/images/gillog/post/b730ee7a-93e1-41d4-bdfa-a9d8b7312dd8/image.png)

위 사진 처럼 **기본 Spring Boot에서 생성과 동시에 나오는 Test Class는 TestClss 명과 `@SpringBootApplication` 으로 생성된 `DdajaApplication` Class 명이 일치**한다.

하지만 아래 사진 처럼 **임의로 작성한 TestClass명은 Class 명이 일치하지 않는다.**

![](https://images.velog.io/images/gillog/post/a0d88396-644d-458b-a71d-280b7375e344/image.png)


# 해결

해결 방법은 **`@SpringBootApplication` Annotation으로 설정한 Class 명을 `@ContextConfiguration(classes = DdajaApplication.class)`
이렇게 TestClass 위에 Annotation을 추가**해주면 된다.
![](https://images.velog.io/images/gillog/post/6e54bfaa-8f0e-4099-92ae-767d899432c2/image.png)


이제 jUnit Test를 마음 껏 누릴 수 있다!

![](https://images.velog.io/images/gillog/post/317e75f5-07a4-4a51-9f59-57b7147c0514/image.png)


오늘도 해결 완료 :) 🙆🏻‍♂️
