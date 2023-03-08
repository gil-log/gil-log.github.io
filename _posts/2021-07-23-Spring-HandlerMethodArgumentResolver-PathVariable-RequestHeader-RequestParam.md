---
title: "[Spring] Resolver 란?  Resolver 구현하기(HandlerMethodArgumentResolver)"
last_modified_at: 2021-07-23T08:26
categories: 
  - spring
tags: 
  - '@PathVariable' 
  - '@RequestParam' 
  - 'Handler Parameter' 
  - 'MethodArgumentResolver' 
  - 'RequestHeader' 
  - 'Spring' 
  - 'resolver'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Custom HandlerMethodArgumentResolver 만들어보기[Advenoh - FRANK OH]](https://blog.advenoh.pe.kr/spring/HandlerMethodArgumentResolver-%EC%9D%B4%EB%9E%80/)

[Spring Argument Resovler[jaehun2841.github.io]](https://jaehun2841.github.io/2018/08/10/2018-08-10-spring-argument-resolver/#spring-argument-resolver)


---

# HandlerMethodArgumentResolver


**`HandlerMethodArgumentResolver`**는 **`Interface`**로써,

**`Controller`의 `Argument(Parameter)`에 지정된 변수들**을,

**`Annotation`이나 객체의 `Type`에 따라**서 **`Resolver`를 먼저 거쳐**,

**실제 Data를 `Controller`에 넘겨주는 역할을 수행**한다.

<br>

**`Controller`에 들어오는 `Argument(Parameter)`를 가공(암호화 > 복호화)** 하거나, 

**`Argument(Parameter)`를 추가하거나 수정해야 하는 경우에 사용**한다.

<br>


**실제 해당 `Interface`의 형태는 아래**와 같다.

```java
/**
 * Strategy interface for resolving method parameters into argument values in
 * the context of a given request.
 *
 * @author Arjen Poutsma
 * @since 3.1
 * @see HandlerMethodReturnValueHandler
 */
public interface HandlerMethodArgumentResolver {

	/**
	 * Whether the given {@linkplain MethodParameter method parameter} is
	 * supported by this resolver.
	 * @param parameter the method parameter to check
	 * @return {@code true} if this resolver supports the supplied parameter;
	 * {@code false} otherwise
	 */
	boolean supportsParameter(MethodParameter parameter);

	/**
	 * Resolves a method parameter into an argument value from a given request.
	 * A {@link ModelAndViewContainer} provides access to the model for the
	 * request. A {@link WebDataBinderFactory} provides a way to create
	 * a {@link WebDataBinder} instance when needed for data binding and
	 * type conversion purposes.
	 * @param parameter the method parameter to resolve. This parameter must
	 * have previously been passed to {@link #supportsParameter} which must
	 * have returned {@code true}.
	 * @param mavContainer the ModelAndViewContainer for the current request
	 * @param webRequest the current request
	 * @param binderFactory a factory for creating {@link WebDataBinder} instances
	 * @return the resolved argument value, or {@code null}
	 * @throws Exception in case of errors with the preparation of argument values
	 */
	Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
			NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception;

}

```

## HandlerMethodArgumentResolver 사용 이유
**`HandlerMethodArgumentResolver`를 사용하는 이유**는,

**매개변수로 사용되는 인자**에 대해 **공통적으로 처리해야할 로직 등이 있을 경우**,

**중복 코드를 줄이고 공통 기능으로 추출하여 사용**할 수 있다.


## 동작 방식

**`Spring`에서 `Resolver`의 동작은 아래와 같은 과정**으로 이루어진다.

![](https://jaehun2841.github.io/2018/08/10/2018-08-10-spring-argument-resolver/Dispatch-Seq.jpg)

1. **`Client Request` 요청**

2. **`Dispatcher Servlet`에서 해당 요청 처리**

3. **`Client Request`에 대한 `Handler Mapping`**
 3.1 **`RequestMapping`에 대한 매칭** (**`RequestMappingHandlerAdapter`가 수행**)
3.2 **`Interceptor` 처리**
3.3 **`Argument Resolver` 처리** <-- **`Argument Resolver` 실행 지점**
3.4 **`Message Converter` 처리**

4. **Controller Method invoke**

>[Spring Argument Resovler[jaehun2841.github.io]](https://jaehun2841.github.io/2018/08/10/2018-08-10-spring-argument-resolver/#spring-argument-resolver)

정리하자면 **특정 `Request`가 `Handler`로 `Mapping`되는 과정에서 `invoke` 되기 전**,

**`Interceptor` > `Resolver` > `MessageConverter` 순으로 처리된 후**,

**`Controller`의 Method가 `invoke`** 된다.

---

# Resolver 구현


먼저 **`HandlerMethodArgumentResolver`를 `implemnets`하는 Class를 생성**한다.

이때 **구현해야 하는 method는 `supportsParameter`, `resolveArgument` 두 가지**이다.

---

## public boolean supportsParameter(MethodParameter parameter)

**`Parameter`가 해당 `Resolver`에 의해 수행 되는 Type인지 체크**하여 **`boolean`을 return**한다.

**`true`로 return될 경우 `resolveArgument` method를 실행**한다.

이때 **`Type` 체크를 위해 `Class`를 `.isAssignableFrom()`을 이용해 비교 할 수도**,

**`Annotation`을 새로 생성하여 체크**할 수도 있다.


<br>


### .isAssignalbeFrom() 이용 Prameter 객체 Class 타입 체크

아래와 같이 **`Parameter`에서 Binding 되길 원하는 객체의 Class Type을 `.isAssignableFrom()`을 이용해 구현**하면,
_[isAssignableFrom](https://velog.io/@gillog/Java-instanceof-Class.isAssignableFrom)이 궁금하다면 클릭_

```java
	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		return ResultJwt.class.isAssignableFrom(parameter.getParameterType());
	}
```

아래에서 처럼 **`Handler`에서 해당 `Parameter` 객체 Class 타입을 사용하는 Parameter가 `Handelr`의 처리 지점**이 된다.

```java
	@GetMapping("")
	public ResponseEntity<Contents<Board>> getNotices(ResultJwt resultJwt,
			@RequestParam(required = false) String category, CommonParameter<NoticeType> parameter) {

		String platformId = (String) resultJwt.getClaims().get(JwtClaims.PLATFORMID.getClaim());
```



<br>

### Annotation 생성

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface ResultJWT {
}
```

**사용된 `Annotation`들의 속성은 아래**와 같다.

- **@interface** : **해당 파일을 `Annotation` Class로 지정**, **`@ResultJWT` 라는 `Annotation`이 생성**됨


- **@Target(ElementType.PARAMETER)** : 해당 **`Annotation`이 생성될 위치 지정**,
**PARAMETER로 지정 시 Method의 Parameter에서만 사용 가능**

- **@Retention(RetentionPolicy.RUNTIME)** : **`Annotation` 유지 정책을 설정**, 
**`RUNTIME`은 Byte Code File까지 `Annotation` 정보를 유지**, 
**`reflection`을 이용 Runtime시에 해당 `Annotation` 정보를 획득**.
_**reflection** : **구체적인 Class Type을 알지 못해도, 그 Class의 method, type, field들에 접근할 수 있도록 해주는 Java API**_


```java
	@GetMapping("")
	public ResponseEntity<Contents<Board>> getNotices(@ResultJWT ResultJwt resultJwt,
			@RequestParam(required = false) String category, CommonParameter<NoticeType> parameter) {

		String platformId = (String) resultJwt.getClaims().get(JwtClaims.PLATFORMID.getClaim());
```

위와 같이 **`@ResultJWT` Annotation 적용 후  supportsParameter()를 아래와 같이 구현** 시,

**해당 Parameter가 Resolver의 처리 지점**이 된다.



```java
    @Override
    public boolean supportsParameter(MethodParameter parameter) {

        boolean isResultJwtAnnotation =  parameter.getParameterAnnotation(ResultJWT.class) != null;

        boolean isResultJwtClass = ResultJwt.class.equals(parameter.getParameterType());

        return isResultJwtAnnotation && isResultJwtClass;
    }

```



---


## public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception

실제 **`Parameter`와 Binding하여 return할 `Object`를 생성하는 method**이다.

**`NativeWebRequest` Object에 접근**하여 **Client Request의 `Parameter`**를 **Controller 보다 우선적으로 받아 작업**할 수 있다.

해당 **`Handler` method안 `Parameter`에서 Binding을 원하는 객체 Type에 맞게 return**해주면 된다.


```java
public class ResultJwtArgumentResolver implements HandlerMethodArgumentResolver {
	@Autowired
	private AuthService authService;

	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		return ResultJwt.class.isAssignableFrom(parameter.getParameterType());
	}

	@Override
	public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest,
			WebDataBinderFactory binderFactory) throws Exception {
                // return type은 본인이 Binding을 원하는 객체 Class
                // supportsParameter에서 검증한 ResultJwt.class
		return authService.getResultJwt(webRequest.getHeader("Authorization"));
	}
}

```

---


위 **두 가지 method를 구현하는 `Class`를 생성해준 후**,

**`servlet-context.xml`에 등록**하거나, **`Configuration`을 이용해 `Resolver`를 등록**해주면 된다.


## servlet-context.xml 등록

```
	<mvc:annotation-driven>
		<mvc:argument-resolvers>
			<bean class="project.config.resolver.ResultJwtArgumentResolver"></bean>
		</mvc:argument-resolvers>
	</mvc:annotation-driven>
```


## Configuration 이용

```java
@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {

    private final ResultJwtArgumentResolver resultJwtArgumentResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(resultJwtArgumentResolver);
    }
}
```

---

# 실제 적용


**실제 본인의 경우**는 **아래와 같이 `@RequestHeader`로 `Header`에서 JWT 토큰을 가져와 `Claims`를 추출하는 공통 로직 부분**을


```java
	@GetMapping("")
	public ResponseEntity<Contents<Board>> getNotices(@RequestHeader(value = "Authorization") String jwt,
			@RequestParam(required = false) String category, CommonParameter<NoticeType> parameter) {

		ResultJwt resultJwt = authService.getResultJwt(jwt);
		String platformId = (String) resultJwt.getClaims().get(JwtClaims.PLATFORMID.getClaim());
```


**간단한 `Resolver`를 구현하여 공통 로직을 추출**하였고,

```java
public class ResultJwtArgumentResolver implements HandlerMethodArgumentResolver {
	@Autowired
	private AuthService authService;

	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		return ResultJwt.class.isAssignableFrom(parameter.getParameterType());
	}

	@Override
	public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest,
			WebDataBinderFactory binderFactory) throws Exception {
		return authService.getResultJwt(webRequest.getHeader("Authorization"));
	}
}

```

아래와 같이 **효과적으로 바로 `Handler`에서 매개변수로 사용할 수 있게 적용해 사용 중**이다.

```java
	@GetMapping("")
	public ResponseEntity<Contents<Board>> getNotices(ResultJwt resultJwt,
			@RequestParam(required = false) String category, CommonParameter<NoticeType> parameter) {
		String platformId = (String) resultJwt.getClaims().get(JwtClaims.PLATFORMID.getClaim());
```


