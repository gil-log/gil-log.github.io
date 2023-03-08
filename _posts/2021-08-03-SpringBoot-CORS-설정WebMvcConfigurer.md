---
title: "[SpringBoot] CORS 설정(WebMvcConfigurer)"
last_modified_at: 2021-08-03T23:02
categories: 
  - spring
tags: 
  - 'Springboot' 
  - 'WebMvcConfigurer' 
  - 'cors'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Spring Boot] CORS 설정하기](https://dev-pengun.tistory.com/entry/Spring-Boot-CORS-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

---


![](https://images.velog.io/images/gillog/post/df21e8d4-05e1-47c2-a664-13334f406878/image.png)

FrontEnd와 BackEnd를 구분지어 개발하거나,

**서로 다른 Server 환경(Port, Domain ...)에서 자원을 공유해야 할 때**,

위 처럼 **CORS 설정이 안되있을 경우 오류가 발생**한다.

이번엔 **Spring Boot에서 `WebMvcConfigurer`를 이용해 간단하게 CORS 설정 하는 방법**을 길록해둔다.

---

방법은 단순하다. 

**`WebMvcConfigurer`를 `implements`**해준다.

```java

@Configuration
public class CorsConfig implements WebMvcConfigurer {
    
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "POST", "PUT", "PATCH", "OPTIONS")
                .allowedHeaders("headers")
                .maxAge(3000);
    }
}

```

위와 같이 작성한 **`CorsConfig` Class를 간단하게 설명**하자면,

**`@Configuration` Annotation을 통해 Container에 Bean 등록** 될 수 있고,

<br>

`CorsRegistry` 객체에 **`.addMapping`을 통해 해당 서버의 모든 `URL` 요청에 해당 Class가 동작**하게 설정,

<br>

**`.allowedOrigins("*")`를 통해 모든 Origin을 허용**해주는데,

```java
.allowedOrigins("https://gillog.com", "localhost:8014")
```
와 같이 **원하는 도메인만 허용해 줄 수도** 있다.

<br>

**`.allowedMethods`를 통해 원하는 `HTTP Methods`를 허용**해주고(GET, POST, PUT, PATCH, DELETE, OPTIONS, 등등 설정 가능)

<br>

**`.allowedHeader`**를 통해 **`Http Request Header`에 허용해줄 Header Name을 설정**해 줄 수 있다.

<br>

**`.maxAge()`로는 PreFilght 요청을 원하는 시간 만큼 캐싱**해줄 수 있다.