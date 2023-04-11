---
title: "Spring에서 Bean의 메모리 생명주기와 GC"
last_modified_at: 2023-04-13T07:54
categories:
  - java
  - spring
tags:
  - Java
  - Spring
  - Bean
  - GC
toc: true
toc_label: "목차"
toc_icon: "sort"
toc_sticky: true
---


`login();`

안녕하세요 Log 입니다.

**Spring에서는 Bean을 IoC 컨테이너에서 관리**합니다. 

**Bean은 `Scope`에 따라 메모리에 상주**하며, **메모리 관리 및 GC와의 관계도 중요**합니다. 

이번 **Post에서는 Bean이 메모리에 상주하는 방식**과 **Scope에 따른 생성, 소멸, 관리 방식**, 

그리고 **GC와의 관계에 대해** 살펴보겠습니다.



# Bean의 메모리 상주

**Bean은 Spring에서 `IoC 컨테이너`에 등록**되어, **해당 컨테이너가 관리**합니다. 

**Bean이 생성된 후, 해당 Bean은 Heap 메모리 영역에 상주**합니다. 

<br>

이때, **Bean의 `Scope`에 따라 메모리에 상주하는 방식이 달라집니다**.


```java
@Component
@Scope("prototype")
public class GillogBean {
    ,,,
}
```


## Singleton Scope


```java
@Component
// singleton scope은 기본값으로 생략가능
// @Scope("singleton")
public class GillogBean {
    ,,,
}
```

**Bean이 한 번 생성되고, 애플리케이션 전체에서 유일하게 사용**됩니다.

따라서 Bean은 **애플리케이션이 종료될 때까지 메모리에 상주**합니다.


Spring에서 `@Scope` Annotation을 사용하지 않으면 **기본적으로 Singleton Scope로 Bean이 생성**됩니다.

**Singleton Scope은 애플리케이션 전체**에서 **하나의 인스턴스만 생성**되며, 

해당 **Bean에 대한 요청이 들어오면 항상 같은 인스턴스가 반환**됩니다. 


따라서 **Singleton Scope은 메모리 사용량을 줄이고**, **애플리케이션 전반에서 동일한 상태를 유지하는 데 유용**합니다. 

하지만, **여러 클라이언트에 대한 요청이 들어올 때 마다 새로운 인스턴스를 생성해야 하는 경우**에는,

`Prototype Scope`를 사용하는 것이 좋습니다.

## Prototype Scope

```java
@Component
@Scope("prototype")
public class GillogBean {
    ,,,
}
```

**Bean이 요청될 때마다 새로 생성**되고, **사용이 끝나면 GC에 의해 메모리에서 해제**됩니다.

## Request Scope


```java
@Component
@Scope("request")
// 4.3 부터 지원
// @RequestScope
public class GillogBean {
    ,,,
}
```

`Request` Scope 에서는 **HTTP 요청 당 하나의 Bean이 생성**되고, 

**요청이 끝날 때 Bean이 GC에 의해 메모리에서 해제**됩니다.


## Session Scope


```java
@Component
@Scope("session")
// @SessionScope
public class GillogBean {
    ,,,
}
```

`Session`의 경우 **HTTP 세션 당 하나의 Bean이 생성**되고, 

**세션이 끝날 때 Bean이 GC에 의해 메모리에서 해제**됩니다.

## GlobalSession Scope

```java
@Component
@Scope("globalSession")
// @SessionScope
public class GillogBean {
    ,,,
}
```

`GlobalSession` Scope의 경우 포털 애플리케이션에서 **전역 세션 당 하나의 Bean이 생성**되고, 

**전역 세션이 끝날 때 Bean이 GC에 의해 메모리에서 해제**됩니다.

전역 세션의 경우, Spring Web MVC에서 `PortletSession`을 확장한 `MultipartPortletSession` 클래스를 사용하여 구현됩니다.

**전역 세션은 일반적인 웹 애플리케이션에서는 사용되지 않는 것이 일반적**입니다. 

**일반적인 웹 애플리케이션에서는 일반 세션을 사용**합니다.


---

# Bean의 생성과 소멸

**Bean은 일반적으로 다음과 같은 생명주기**를 가집니다.

- Bean의 생성
- Bean의 속성 값 주입
- Bean의 초기화
- Bean의 사용
- Bean의 소멸


## Bean의 생성

**Bean의 생성**은 `@Bean` Annotation이나 XML 파일을 이용해서 등록된 설정 정보를 기반으로 **IoC 컨테이너가 생성**합니다. 

## Bean의 속성 값 주입

**속성 값 주입은 생성된 Bean에 대해 `DI(Dependency Injection)`를 수행**합니다. 

## Bean의 초기화

초기화는 `@PostConstruct` Annotation이나 `InitializingBean` interface를 이용해서 Bean 초기화 작업을 수행합니다.

**초기화는 모두 Bean이 생성되어 속성 값이 주입된 이후 동작**됩니다.

### @PostConstruct 초기화 방법

`@PostConstruct` Annotation을 이용해서 Bean 초기화 작업을 수행하는 예제는 아래와 같습니다.

```java
@Component
public class ExampleBean {

    private String logMessage;

    public void setLogMessage(String logMessage) {
        this.logMessage = logMessage;
    }

    public String getLogMessage() {
        return logMessage;
    }

    @PostConstruct
    public void init() {
        // Bean 초기화 작업
        System.out.println("Bean Initialized: " + logMessage);
    }
}

```
`@PostConstruct` Annotation이 붙은 `init()` method는 Bean의 초기화 작업을 수행합니다. 

이 method는 **Bean이 생성된 후, `DI(Dependency Injection)`가 완료**되고, **속성값이 모두 설정된 후 호출**됩니다.


### InitializingBean interface implements 초기화 방법

`InitializingBean` interface를 이용해서 Bean의 초기화 작업을 수행할 수 있습니다. 

`InitializingBean` interface를 구현한 클래스는 `afterPropertiesSet()` method를 오버라이드해서,

초기화 작업을 수행합니다.

```java
@Component
public class ExampleBean implements InitializingBean {

    private String logMessage;

    public void setLogMessage(String logMessage) {
        this.logMessage = logMessage;
    }

    public String getLogMessage() {
        return logMessage;
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        // Bean 초기화 작업
        System.out.println("Bean Initialized: " + logMessage);
    }
}
```

`afterPropertiesSet()` method는 `InitializingBean` interface의 method로서, 

Bean의 초기화 작업을 수행합니다. 


이 method 역시 **Bean이 생성된 후, DI(Dependency Injection)가 완료된 후 호출**됩니다.

## Bean의 사용

**Bean을 사용하는 행위는 DI를 통해 다른 Bean에서 사용**됩니다.

_우리가 흔히 `@Autowired`, `생성자 주입 방식` 등으로 주입_

```java
@RequiredArgsConstructor
public class ExampleService {
    
    private final ExampleBean exampleBean;
    
    ,,,
}
```


## Bean의 소멸

Bean의 소멸은 `@PreDestroy` Annotation이나, `DisposableBean` interface를 이용해서 **Bean이 소멸 되기 전 수행**됩니다.


### @PreDestroy 소멸 방법

`@PreDestroy` Annotation을 이용해서 Bean 소멸 작업을 수행하는 예제는 아래와 같습니다.

```java
@Component
public class ExampleBean {

    private String logMessage;

    public void setLogMessage(String logMessage) {
        this.logMessage = logMessage;
    }

    public String getLogMessage() {
        return logMessage;
    }

    @PreDestroy
    public void destroy() {
        // Bean 소멸 작업
        System.out.println("Bean Destroyed: " + logMessage);
    }
}
```

`@PreDestroy` annotation이 붙은 `destroy()` method는 Bean의 소멸 작업을 수행합니다.

이 **method는 Bean이 소멸되기 전에 호출**됩니다.

### DisposableBean interface implements  초기화 방법

`DisposableBean` interface를 구현한 클래스는 `destroy()` method를 오버라이드해서 소멸 작업을 수행합니다.

```java
@Component
public class ExampleBean implements DisposableBean {

    private String logMessage;

    public void setLogMessage(String logMessage) {
        this.logMessage = logMessage;
    }

    public String getLogMessage() {
        return logMessage;
    }

    @Override
    public void destroy() throws Exception {
        // Bean 소멸 작업
        System.out.println("Bean Destroyed: " + logMessage);
    }
}
```
`DisposableBean` interface를 구현한 `destroy()` method는 Bean의 소멸 작업을 수행합니다.

이 method는 Bean이 소멸되기 전에 호출됩니다.



---

# Bean의 Scope와 GC

**Bean의 Scope는 Bean이 메모리에 상주하는 시간을 결정**합니다. 

이에 따라 GC와의 관계도 달라집니다. 

**`Singleton Scope`의 Bean은 애플리케이션 전체에서 유일하게 사용**됩니다. 

따라서 **메모리에서 해제되지 않습니다.** 

`Prototype Scope`의 Bean은 **요청될 때마다 새로 생성**되고,

**사용이 끝나면 GC에 의해 메모리에서 해제**됩니다. 

`Request Scope`, `Session Scope`, `Global session Scope`의 Bean은,

각각 **Http Request, 세션, 전역 세션 단위로 생성**되고, **해당 단위가 끝날 때 GC에 의해 메모리에서 해제**됩니다.

Bean이 메모리에서 해제되기 위해서는 Bean을 참조하고 있는 다른 객체가 없어야 합니다. 

따라서 **Bean이 참조하는 다른 Bean이나, Bean을 이용하는 다른 객체가 메모리에서 해제되기 전**에는,

**해당 Bean이 GC에 의해 해제되지 않습니다.**

---

# 결론

이번 글에서는 **Bean이 어떤 상황에서 메모리에 상주**하며,

**어떤 상황에서 GC에 의해 메모리에서 해제**되는지 알아보았습니다. 

**Bean의 Scope와 GC와의 관계를 고려하여 개발을 진행**하면,

좀 더 **안정적이고 효율적인 애플리케이션을 개발**할 수 있습니다.

이번 글이 **Bean의 Scope에 따른 메모리 상주와 GC와의 관계**를 이해하는 데 도움이 되었기를 바랍니다.

감사합니다.

`logout();`
