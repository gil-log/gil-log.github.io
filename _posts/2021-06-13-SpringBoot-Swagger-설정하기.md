---
title: "[SpringBoot] Swagger 설정하기"
last_modified_at: 2021-06-13T23:05
categories: 
  - spring
tags: 
  - 'Springboot' 
  - 'Swagger'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Spring Boot / STS] 스프링부트에 swagger 연동 및 사용법[haddoddo]](https://haddoddo.tistory.com/entry/Spring-Boot-STS-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8%EC%97%90-swagger-%EC%97%B0%EB%8F%99-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95)

[[Spring Boot] Swagger 연동 및 설정 방법[Bamdule]](https://bamdule.tistory.com/36)

---

# Swagger

**`Swagger`**란 **서버로 요청되는 URL 리스트**를 **HTML화면으로 문서화 및 테스트 할 수 있는 라이브러리**이다.

간단하게 설명하면 `Swagger`는 `API Spec 문서`이다.

**API를 엑셀이나 가이드 문서를 통해 관리하는 방법**은 **주기적인 업데이트가 필요**하기 때문에 **관리가 쉽지 않고 시간이 오래** 걸린다.

그래서 **`Swagger`를 사용해 `API Spec 문서`를 자동화**해주어 **간편하게 API문서를 관리하면서 테스트**할 수 있다.

---

# Swagger 설정하기

먼저 **`Swagger` 사용을 위해 `pom.xml`에 dependency를 추가**한다.

```
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.9.2</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.9.2</version>
</dependency>
```


![](https://images.velog.io/images/gillog/post/6d952347-f781-4996-86c0-209c300acd22/image.png)


그 후 환경 설정을 위해 `SwaagerConfig` Class를 만들어준다.

```java
package com.ddaja.config;

import java.util.HashSet;
import java.util.Set;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .consumes(getConsumeContentTypes())
                .produces(getProduceContentTypes())
                .apiInfo(getApiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.ddaja.ddaja.controller"))
                .paths(PathSelectors.ant("/**"))
                .build();
    }

    private Set<String> getConsumeContentTypes() {
        Set<String> consumes = new HashSet<>();
        consumes.add("application/json;charset=UTF-8");
        consumes.add("application/x-www-form-urlencoded");
        return consumes;
    }

    private Set<String> getProduceContentTypes() {
        Set<String> produces = new HashSet<>();
        produces.add("application/json;charset=UTF-8");
        return produces;
    }

    private ApiInfo getApiInfo() {
        return new ApiInfoBuilder()
                .title("API")
                .description("[DDaJa] REST API")
                .contact(new Contact("[DDaja Swagger]", "https://github.com/swgil007/DDaJa", "BNG"))
                .version("1.0")
                .build();
    }
}
```

## Swagger Config 설명

`.consume()`과 `.produces()`는 각각 `Request Content-Type`, `Response Content-Type`에 대한 설정. (선택)

`.apiInfo()`는 **Swagger API 문서에 대한 설명을 표기하는 메소드**. (선택)

`.apis()`는 **Swagger API 문서로 만들기 원하는 BasePackage 경로**. (필수)

`.path()`는 **URL 경로를 지정하여 해당 URL에 해당하는 요청만 Swagger API 문서**로 만든다. (필수)

---

# Swagger 사용하기

이제 **`Swaager`를 사용**할 수 있는데, **아래 Annotations들을 통해서 간단하게 설명 문구들이나 설정**을 마치고, 

**`localhost/swagger-ui.html` url 경로를 통해서 `Swaager API 문서` 페이지**를 볼 수 있다.

## @ApiOperation

**`@ApiOperation` Annotation**은 value, notes 를 통해서 **해당 API에 대한 설명이나 설정**을 진행할 수 있다.



## @ApiParam

**`@ApiParam` Annotation 으로 DTO 변수에 대한 설명 및 다양한 설정을 사용**할 수 있다.

![](https://images.velog.io/images/gillog/post/d124b8e7-e1ce-4064-91f3-05695f9bc091/image.png)

