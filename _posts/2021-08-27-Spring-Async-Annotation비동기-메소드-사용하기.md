---
title: "[Spring] @Async Annotation(비동기 메소드 사용하기)"
last_modified_at: 2021-08-27T00:05
categories: 
  - spring
tags: 
  - 'Spring' 
  - 'async' 
  - '비동기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[springboot 비동기 서비스 만들기(Async)-Hanumoka, IT Blog](https://www.hanumoka.net/2020/07/02/springBoot-20200702-sringboot-async-service/)

[How does @Async work? @Async를 지금까지 잘 못 쓰고 있었습니다(@Async 사용할 때 주의해야 할 것, 사용법)[기본기를 쌓는 정아마추어 코딩블로그]](https://jeong-pro.tistory.com/187)

[Effective Advice on Spring Async: Part 1[DZone]](https://dzone.com/articles/effective-advice-on-spring-async-part-1)

[]()

[]()

---

# @Async

**`@Async` Annotation**은 **Spring에서 제공**하는 **Thread Pool을 활용하는 비동기 메소드 지원 Annotation**이다.

기존 **Java에서 비동기 방식으로 메소드를 구현할 때는 아래와 같이 구현**할 수 있었다.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class GillogAsync {

    static ExecutorService executorService = Executors.newFixedThreadPool(5);

    public void asyncMethod(final String message) throws Exception {
        executorService.submit(new Runnable() {
            @Override
            public void run() {
                // do something
            }            
        });
    }
}
```
**`java.util.concurrent.ExecutorService`을 활용**해서 **비동기 방식의 method를 정의 할 때마다**,

위와 같이 **`Runnable`의 `run()`을 재구현해야 하는 등 동일한 작업들의 반복**이 잦았다.


## With @Async

**`@Async` Annotation을 활용하면 손쉽게 비동기 메소드 작성이 가능**하다.

**만약 `Spring Boot`에서 간단히 사용**하고 싶다면, 단순히 **`Application` Class에 `@EnableAsync` Annotation을 추가**하고,

```java
@EnableAsync
@SpringBootApplication
public class SpringBootApplication {
    ...
}
```

**비동기로 작동하길 원하는 method 위에 `@Async` Annotation을 붙여주면 사용**할 수 있다.

```java
public class GillogAsync {

    @Async
    public void asyncMethod(final String message) throws Exception {
        ....
    }
}
```

위와 같은 **사용은 간단하지만 `@Async`의 기본설정인 `SimpleAsyncTaskExecutor`를 사용**한다.

![](https://images.velog.io/images/gillog/post/37347b0e-42d5-47db-ad12-4267fa101838/image.png)

**본인의 개발 환경에 맞게 Customize하기에는 직접 `AsyncConfigurerSupport`를 상속받는 Class를 작성**하는 것이 좋다.

## AsyncConfigurerSupport

**아래와 같은 `AsyncConfigurerSupport`를 상속받는 Customize Class를 구현**하자.

```java
import java.util.concurrent.Executor;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.AsyncConfigurerSupport;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
    
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(30);
        executor.setQueueCapacity(50);
        executor.setThreadNamePrefix("DDAJA-ASYNC-");
        executor.initialize();
        return executor;
    }
}
```

여기서 **설정한 요소들은 아래**와 같다.

- **@Configuration** : **Spring 설정 관련 Class로 @Component 등록되어 Scanning** 될 수 있다.
- **@EnableAsync** : **Spring method에서 비동기 기능을 사용가능하게 활성화** 한다.
- **CorePoolSize : **기본 실행 대기하는 Thread의 수**
- **MaxPoolSize : 동시 동작하는 최대 Thread의 수**
- **QueueCapacity** : **MaxPoolSize 초과 요청**에서 **Thread 생성 요청시**, 
**해당 요청을 Queue에 저장**하는데 이때 **최대 수용 가능한 Queue의 수**,
Queue에 저장되어있다가 **Thread에 자리가 생기면 하나씩 빠져나가 동작**
- **ThreadNamePrefix** : **생성되는 Thread 접두사** 지정




<br>

위와 같이 작성한 후 **비동기 방식 사용을 원하는 method에 `@Async` Annotation을 지정**해주면 된다.

---

# 주의사항

**`@Async` Annotation을 사용할 때 아래와 같은 세 가지 사항을 주의**하자.

1. **private method는 사용 불가**

2. **self-invocation(자가 호출) 불가**, 즉 **inner method는 사용 불가**

3. **QueueCapacity 초과 요청에 대한 비동기 method 호출시 방어 코드 작성**


**위 주의사항을 아래 사진과 함께 설명**을 해보면,

![](https://images.velog.io/images/gillog/post/5bb64a29-5263-4fcc-9f02-cffea4162137/image.png)

> 출처 : https://dzone.com/articles/effective-advice-on-spring-async-part-1


**`@Async`의 동작**은 **AOP가 적용**되어 **`Spring Context`에서 등록된 Bean Object의 method가 호출 될 시**에,

**Spring이 확인**할 수 있고 **`@Async`가 적용된 method**의 경우 **Spring이 method를 가로채 다른 Thread에서 실행 시켜주는 동작 방식**이다.

이 때문에 **Spring이 해당 `@Async` method를 가로챈 후, 다른 Class에서 호출이 가능해야** 하므로,

**`private` method는 사용할 수 없는 것**이다.

<br>

또한 **`Spring Context`에 등록된 `Bean`의 method의 호출이어야 `Proxy` 적용이 가능**하므로,

**inner method의 호출은 `Proxy` 영향을 받지 않기에 `self-invocation`이 불가능**하다.

위 주의사항을 아래 예시 코드와 함께 살펴보자

## self-invocation(자가 호출) 불가


위에서 작성한 `AsyncConfig`를 사용하는 Spring Project에서 아래와 같이,

**같은 Class에 존재하는 method에 `@Async` Annotation을 작성해 비동기 방식을 사용**해보자.

```java
@Controller
public Class TestController {

    @Async
    public void asyncMethod(int i) {
        try {
            Thread.sleep(500);
            log.info("[AsyncMethod]"+"-"+i);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
    }

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        for(int i=0; i<50; i++) {
            asyncMethod(i);
        }
        return "";
    }
}
```

![](https://images.velog.io/images/gillog/post/925c2314-0f9a-43b6-978b-bb5f6eef6dd7/image.png)

**작동 결과를 보면 비동기 방식으로 호출되지 않았고, 동기적으로 호출 순서대로 동작하는 것을 확인**할 수 있다.

**자가 호출에서는 `@Async` 사용이 불가**하다.

<br>

하지만, **`@Service`로 Bean 등록된 Service를 통해 주입하여 위 코드를 다시 작성**해보면,


```java
@Service
public class TestService {
    @Async
    public void asyncMethod(int i) {
        try {
            Thread.sleep(500);
            log.info("[AsyncMethod]"+"-"+i);
        } catch(InterruptedException e) {
            e.printStackTrace();
        }
    }
}

@AllArgsConstructor
@Controller
public Class TestController {

    private TestService testService;

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        for(int i=0; i<50; i++) {
            testService.asyncMethod(i);
        }
        return "";
    }
    
}
```

![](https://images.velog.io/images/gillog/post/97f0ec1f-c0f3-4d2f-b539-c573524b6707/image.png)

위 사진과 같이 **호출 순서에 상관없이 비동기 방식으로 method가 호출** 되었고,

**`AsyncConfig`에서 prefix로 작성한 접두사도 정상적으로 붙은 것을 확인**할 수 있다.



## QueueCapacity 초과 요청 방어 코드 작성

이번엔 **`AsyncConfig`에서 `PoolSize`와 `QueueCapacity`를 줄여보고 위 코드를 다시 실행**해보자.

```java
@Configuration
@EnableAsync
public class AsyncConfig extends AsyncConfigurerSupport {
    
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(2);
        executor.setMaxPoolSize(10);
        executor.setQueueCapacity(10);
        executor.setThreadNamePrefix("DDAJA-ASYNC-");
        executor.initialize();
        return executor;
    }
}
```

**Exception이 Throw** 되었다.

![](https://images.velog.io/images/gillog/post/7503cd88-fa9b-4d4b-b014-8590ffe0a722/image.png)

Exception에서 주요한 사항을 살펴보면

```
Request processing failed; nested exception is org.springframework.core.task.TaskRejectedException: 
Executor [java.util.concurrent.ThreadPoolExecutor@116e53a0
[Running, pool size = 10, active threads = 10, queued tasks = 10, completed tasks = 0]] 
did not accept task: org.springframework.aop.interceptor.AsyncExecutionInterceptor$$
Lambda$2031/170931344@7ef0a05e] with root cause
```

**`TaskRejectedException`**으로 수행된 task는 0으로 설정 thread 수, pool size 는 10, 

queued 된 tasks = 10개로 **최대 수용 가능한 Thread Pool 수와 QueueCapacity 까지 초과된 요청**이 들어오자,

**Task를 Reject하는 Exception이 발생**하였다.

아래와 같이 **`TaskRejectedException` 발생 시 handling 해주는 방어 코드를 작성**하자.

```java
@AllArgsConstructor
@Controller
public Class TestController {

    private TestService testService;

    @GetMapping("async")
    public String testAsync() {
        log.info("TEST ASYNC");
        try {
            for(int i=0; i<50; i++) {
                testService.asyncMethod(i);
        } catch (TaskRejectedException e) {
            // ....
        }
        return "";
    }
    
}
```