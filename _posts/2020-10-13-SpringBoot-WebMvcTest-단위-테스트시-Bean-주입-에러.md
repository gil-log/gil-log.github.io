---
title: "[SpringBoot] @WebMvcTest 단위 테스트시 Bean 주입 에러"
last_modified_at: 2020-10-13T08:51
categories: 
  - error
tags: 
  - '에러'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
스프링 부트에서 JUnit @WebMvcTest로 컨트롤러 테스트를 하고 있었는데, 아래와 같은 에러가 발생했다.

```
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userController': Unsatisfied dependency expressed through field 'userRoleService'; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'com.noryangjin.boostcourse.service.UserRoleService' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:643)
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:130)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor
```

![](https://images.velog.io/images/gillog/post/bc3c6127-6d23-401d-a8f9-9daa21c0ac67/bandicam%202020-10-13%2017-36-48-600.png)


확인해 본 결과 해당 **컨트롤러를 생성하는데 필요한 빈을 정의 할 수 없다는 문제** 였다.

**@WebMvcTest는 웹과 관련된 빈만 주입**되고(@Controller, @ControllerAdvice 등)

@Service같은 **@Component는 주입되지 않는다**고 한다.

**@Service 빈이 주입되지 않아, 컨트롤러 생성에 실패하여 발생한 에러**였다.



![](https://images.velog.io/images/gillog/post/c6b540d7-f8ce-4ece-8b16-51449904632d/bandicam%202020-10-13%2017-42-00-696.png)

확인해보니 UserController 에서는 userRoleService가 주입 되어 있는데,

![](https://images.velog.io/images/gillog/post/1ac03e24-04de-462a-902e-96ade02c36a2/bandicam%202020-10-13%2017-40-56-619.png)


@Test에서는 userService만 @MockBean으로 가짜 객체를 주입시켜주고, userRoleService는 의존성 주입을 해주지 않아 발생한 오류 였다.

![](https://images.velog.io/images/gillog/post/a616d134-3b86-4e80-9726-c0e4e1251520/bandicam%202020-10-13%2017-44-31-741.png)

결국 **TestController에 `@MockBean`을 통해 `userRoleService`를 주입시켜주니 해결**됐다!

이렇게 @WebMvcTest를 사용하여 Controller를 Test할때는 아래 내용들만 스캔하도록 제한 된다고 한다.

```
@Controller, @ControllerAdvice, @JsonComponent, 
Converter, GenericConverter, Filter, HandlerInterceptor,
WebMvcConfigurer, HandlerMethodArgumentResolver
```
 
 
 <br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[spring] 스프링 부트에서 @WebMvcTest 사용하며 RestTemplate 주입받기[깜비 님]](https://kkambi.tistory.com/154)

[[스프링부트 (10)] SpringBoot Test(3) - 단위 테스트(@WebMvcTest, @DataJpaTest, @RestClientTest 등)[갓대희 님]](https://goddaehee.tistory.com/212)
