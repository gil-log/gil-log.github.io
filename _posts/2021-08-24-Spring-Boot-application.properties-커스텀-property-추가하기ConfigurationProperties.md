---
title: "[Spring Boot] application.properties 커스텀 property 추가하기(@ConfigurationProperties)"
last_modified_at: 2021-08-24T21:08
categories: 
  - spring
tags: 
  - '@ConfigurationProperties' 
  - 'Spring boot' 
  - 'application.properties' 
  - 'customize property'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[스프링 부트 커스텀 설정 프로퍼티 클래스 사용하기[Java Can Do IT]](https://javacan.tistory.com/entry/springboot-configuration-properties-class)


---

# application.properties

**`Spring Boot`**는 **`application.properties`를 이용한 프로젝트 환경 설정을 제공**한다.

![](https://images.velog.io/images/gillog/post/7955dba1-9be2-4a97-b26a-cf517c2e7780/image.png)

**`Spring Boot`에서 기본적으로 제공**하거나, **각 `Dependency`에서 제공하는 `Property` 외**에도,

개발자가 **직접 `Customize`한 `Property` 역시 설정 가능**하다.

**서버에서 사용하는 `Public Key`값을 관리**한다거나,

**서버 마다 사용하는 로직 변수 값들**을 **`application.properties`를 활용해서 각기 다르게 설정**할 수도 있다.

---

# @ConfigurationProperties Class 생성


**`@ConfigurationProperties`를 활용**하면 손 쉽게 `Spring Boot`의 **`application.properties`에서 사용할 `Property Class`를 생성**할 수 있다.

아래 코드를 살펴보자.

```java
import org.springframework.boot.context.properties.ConfigurationProperties;

import lombok.Getter;
import lombok.Setter;

@ConfigurationProperties(prefix = "eval.publickey")
@Getter
@Setter
public class PublicKeyConfig {
    private String publicKey;
}
```

![](https://images.velog.io/images/gillog/post/9e02c4bc-0eb8-442e-89fb-234d8ae97d49/image.png)

**`@ConfigurationProperties`를 선언한 Class**에서는,

**`application.properties`에서 사용할 `Property`의 Getter, Setter 부분을 작성**해주기만 하면 된다.


## Property Mapping


**`@ConfigurationProperties`를 선언한 Class**에서,

**field로 선언한 변수는 `application.properties`에서 다양하게 매핑 가능**하다.



**기본 적으로는 아래와 같이 매핑** 된다.

* prefix + "." + setter Property의 이름
_[EX] eval.publickey.publicKey_



만약 **`setter`의 `Property 이름` 중간에 대문자가 포함**되어 있다면,

**아래와 같이도 매핑**할 수 있다.

```
eval.publickey.publicKey
eval.publickey.public-key
eval.publickey.public_key
EVAL_PUBLICKEY_PUBLIC_KEY
```

이제 **생성한 `@ConfigurationProperties` Class를 `Bean`으로 등록**해주면 된다.

---

# Bean 등록 하기

**`@ConfigurationProperties` Class**는 아래 **두 가지 방법 중 하나로 Bean으로 등록**해주면 된다.

- @EnableConfigurationProperties
- @Component


## @EnableConfigurationProperties로 등록

**`@EnableConfigurationProperties`**로 Bean 등록 하려면 아래와 같이,

**Spring Boot Application Class**에 **`@EnableConfigurationProperties(PublicKeyConfig.class)`와 같이 선언**해주면 된다.


![](https://images.velog.io/images/gillog/post/b160ae8a-e9a5-4bd5-9f93-1c5bf96d5a44/image.png)


```java
import com.bng.ddaja.common.config.PublicKeyConfig;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;

@SpringBootApplication
@EnableConfigurationProperties(PublicKeyConfig.class)
public class DdajaApplication {
	public static void main(String[] args) {
		SpringApplication.run(DdajaApplication.class, args);
	}
}
```

여기서 `@EnableConfigurationProperties`안의 `PublicKeyConfig.class`는,

아까전 작성한 **`@ConfigurationProperties` Annotation이 적용된 Class**이다.


**`@EnableConfigurationProperties` Annotation**은,

**해당 Class를 Bean으로 등록하고 Property 값을 할당**해준다.

## @Component 등록

**`@Component` Annotation으로 등록**하는 방법은,

아까전 **`@ConfigurationProperties` Class에,
`@Component` Annotation을 작성**해주기만 하면 된다.


![](https://images.velog.io/images/gillog/post/84246e58-5ad1-46f0-b3ba-8f387e233eb5/image.png)

```java

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
@Component
@ConfigurationProperties(prefix = "eval.publickey")
public class PublicKeyConfig {
    private String publicKey;
}

```

**`Spring Boot`의 `
@SpringBootApplication`이 선언된 Class** 하위에,

**`@Component` Type의 Annotation이 붙은 Class**는,

**자동으로 Bean으로 등록해주기 때문에 손쉽게 사용 가능**하다.


---

# 실제 사용해보기

**`@ConfigurationProperties` Annotation Class를 작성**후 **Bean 등록**을 해주고,

![](https://images.velog.io/images/gillog/post/84246e58-5ad1-46f0-b3ba-8f387e233eb5/image.png)



이제 아래와 **`application.properties`에 원하는 `Property`를 설정**하면,

![](https://images.velog.io/images/gillog/post/56e30935-b72a-4b13-a393-a2ed3447da01/image.png)


아래와 같이 **Dependency Injection을 통해**,

**`@ConfigurationProperties` Annotation Class 사용을 원하는 곳에 의존성을 주입**해주면,


```java
@CrossOrigin(origins = "*", allowedHeaders = "*")
@AllArgsConstructor
@RequestMapping("test")
@RestController
@Slf4j
public class TestController {
    
    private PublicKeyConfig publicKeyConfig;

    @GetMapping("/publicKey")
    public String testPublicKeyProperty() {
        return publicKeyConfig.getPublicKey();
    }
}
```

![](https://images.velog.io/images/gillog/post/0b861da6-9779-4d16-91a9-a25a59b73ba3/image.png)

손쉽게 사용할 수 있다.