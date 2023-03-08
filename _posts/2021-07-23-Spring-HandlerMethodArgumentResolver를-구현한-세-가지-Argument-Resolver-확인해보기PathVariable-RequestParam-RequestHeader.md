---
title: "[Spring] HandlerMethodArgumentResolver를 구현한 세 가지 Argument Resolver 확인해보기(@PathVariable, @RequestParam, @RequestHeader)"
last_modified_at: 2021-07-23T09:19
categories: 
  - spring
tags: 
  - '@PathVariable' 
  - '@RequestParam' 
  - 'HandlerMethodArgumentResolver' 
  - 'PathVariableMethodArgumentResolver' 
  - 'RequestHeader' 
  - 'RequestHeaderMapMethodArgumentResolver' 
  - 'RequestParamMethodArgumentResolver' 
  - 'Spring'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Custom HandlerMethodArgumentResolver 만들어보기[Advenoh - FRANK OH]](https://blog.advenoh.pe.kr/spring/HandlerMethodArgumentResolver-%EC%9D%B4%EB%9E%80/)

[Spring Argument Resovler[jaehun2841.github.io]](https://jaehun2841.github.io/2018/08/10/2018-08-10-spring-argument-resolver/#spring-argument-resolver)


---


# Spring에서 구현된 HandlerMethodArgumentResolver

<br>

지금까지 많이 사용했던 **`@PathVariable`, `@RequestParam`, `@RequestHeader` `Annotation` 들**도,

**[HandlerMethodArgumentResolver](https://velog.io/@gillog/Spring-HandlerMethodArgumentResolver-PathVariable-RequestHeader-RequestParam)를 `Spring`에서 구현**한 **`Resolver`에서 사용하게되는 `Annotation`**이다.

<br>

**`Spring`에서 `HandlerMethodArgumentHandler`를 구현한 `Resolver`**로는 **아래 세 가지**,

**`PathVariableMethodArgumentResolver`, `RequestParamMethodArgumentResolver`, `RequestHeaderMapMethodArgumentResolver`**가 있다.

---

## PathVariableMethodArgumentResolver

**`PathVariableMethodArgumentResolver`**는 **`HandlerMethodArgumentHandler` `Interface`를 `implements`**한,

**`AbstractNamedValueMethodArgumentResolver` `Abstract class`**를 **상속한 `Resolver`**이다.

![](https://images.velog.io/images/gillog/post/0786b12c-ce36-487e-8979-317c719d1aba/image.png)


![](https://images.velog.io/images/gillog/post/82e5550d-4790-4fd3-8ffa-c45cbdbd5eb8/image.png)

**`PathVariableMethodArgumentResolver`**는 **`Controller` `Argument` 중**,

**`@PathVariable`이 앞에 선언된 `Argument`**를 **처리하는  `Argument Resolver`**이다.



```java
@GetMapping("/gillog/{id}")
public ResponseEntity<String> getGillog(@PathVariable(name = "id") String id) {
	return ResponseEntity.ok().body(id);
}
```

**`PathVariableMethodArgumentResolver`의 `supportsParameter` method 부분**을 살펴보면,

**parameter의 `Annotation`이 `PathVariable`인지를 체크**하여, **해당 `Resolver`에서 처리 여부를 결정**한다.

```java
	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		if (!parameter.hasParameterAnnotation(PathVariable.class)) {
			return false;
		}
		if (Map.class.isAssignableFrom(parameter.nestedIfOptional().getNestedParameterType())) {
			String paramName = parameter.getParameterAnnotation(PathVariable.class).value();
			return StringUtils.hasText(paramName);
		}
		return true;
	}
```


**위와 같이 `@PathVariable`과 사용되는 Argument를 처리하는 `Resolver`**이고,

**`Spring`에서 구현한 `@PathVariable` Annotation은 아래와 같은 형태**이다.



```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface PathVariable {

	/**
	 * Alias for {@link #name}.
	 */
	@AliasFor("name")
	String value() default "";

	/**
	 * The name of the path variable to bind to.
	 * @since 4.3.3
	 */
	@AliasFor("value")
	String name() default "";

	/**
	 * Whether the path variable is required.
	 * <p>Defaults to {@code true}, leading to an exception being thrown if the path
	 * variable is missing in the incoming request. Switch this to {@code false} if
	 * you prefer a {@code null} or Java 8 {@code java.util.Optional} in this case.
	 * e.g. on a {@code ModelAttribute} method which serves for different requests.
	 * @since 4.3.3
	 */
	boolean required() default true;

}
```


---


## RequestParamMethodArgumentResolver

**`RequestParamMethodArgumentResolver`**은  **`HandlerMethodArgumentHandler` `Interface`를 `implements`**한,

**`AbstractNamedValueMethodArgumentResolver` `Abstract class`**를 **상속한 `Resolver`**이다.



![](https://images.velog.io/images/gillog/post/0786b12c-ce36-487e-8979-317c719d1aba/image.png)


![](https://images.velog.io/images/gillog/post/9bc1ca62-0820-4ba6-8820-699c1a8e672d/image.png)



**`RequestParamMethodArgumentResolver`**는 **`Controller` `Argument` 중**,

**`@RequestParam`이 앞에 선언된 `Argument`**를 **처리하는  `Argument Resolver`**이다.



```java
@GetMapping("/gillog")
public ResponseEntity<String> getGillog(@RequestParam(name = "id") String id) {
	return ResponseEntity.ok().body(id);
}
```

**`RequestParamMethodArgumentResolver`의 `supportsParameter` method 부분**을 살펴보면,

**parameter의 Annotation이 `RequestParam`인지를 체크**하여 **해당 `Resolver`에서 처리할지 결정**한다.

```java
	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		if (parameter.hasParameterAnnotation(RequestParam.class)) {
			if (Map.class.isAssignableFrom(parameter.nestedIfOptional().getNestedParameterType())) {
				String paramName = parameter.getParameterAnnotation(RequestParam.class).name();
				return StringUtils.hasText(paramName);
			}
			else {
				return true;
			}
		}
		else {
			if (parameter.hasParameterAnnotation(RequestPart.class)) {
				return false;
			}
			parameter = parameter.nestedIfOptional();
			if (MultipartResolutionDelegate.isMultipartArgument(parameter)) {
				return true;
			}
			else if (this.useDefaultResolution) {
				return BeanUtils.isSimpleProperty(parameter.getNestedParameterType());
			}
			else {
				return false;
			}
		}
	}
```


**위와 같이 `@RequestParam`과 사용되는 Argument를 처리하는 `Resolver`**이고,

**`Spring`에서 구현한 `@RequestParam` Annotation은 아래와 같은 형태**이다.



```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {

	/**
	 * Alias for {@link #name}.
	 */
	@AliasFor("name")
	String value() default "";

	/**
	 * The name of the request parameter to bind to.
	 * @since 4.2
	 */
	@AliasFor("value")
	String name() default "";

	/**
	 * Whether the parameter is required.
	 * <p>Defaults to {@code true}, leading to an exception being thrown
	 * if the parameter is missing in the request. Switch this to
	 * {@code false} if you prefer a {@code null} value if the parameter is
	 * not present in the request.
	 * <p>Alternatively, provide a {@link #defaultValue}, which implicitly
	 * sets this flag to {@code false}.
	 */
	boolean required() default true;

	/**
	 * The default value to use as a fallback when the request parameter is
	 * not provided or has an empty value.
	 * <p>Supplying a default value implicitly sets {@link #required} to
	 * {@code false}.
	 */
	String defaultValue() default ValueConstants.DEFAULT_NONE;

}

```



---


## RequestHeaderMapMethodArgumentResolver

**`RequestHeaderMapMethodArgumentResolver`**은 **`HandlerMethodArgumentHandler` `Interface`를 `implements`한 `Resolver`**이다.




![](https://images.velog.io/images/gillog/post/eff568f3-445a-45f9-ac06-716a7e5fd6ff/image.png)


**`RequestHeaderMapMethodArgumentResolver`**는 **`Controller` `Argument` 중**,

**`@RequestHeader`가 앞에 선언된 `Argument`**를 **처리하는  `Argument Resolver`**이다.



```java
@GetMapping("/gillog")
public ResponseEntity<String> getGillog(@RequestHeader(name = "Authorization") String jwt) {
	return ResponseEntity.ok().body(jwt);
}
```

**`RequestHeaderMapMethodArgumentResolver`의 `supportsParameter` method 부분**을 살펴보면,

**parameter의 Annotation이 `RequestHeader`인지를 체크**하여 **해당 `Resolver`에서 처리할지 결정**한다.


```java
	@Override
	public boolean supportsParameter(MethodParameter parameter) {
		return (parameter.hasParameterAnnotation(RequestHeader.class) &&
				Map.class.isAssignableFrom(parameter.getParameterType()));
	}
```


**위와 같이 `@RequestHeader`와 사용되는 Argument를 처리하는 `Resolver`**이고,

**`Spring`에서 구현한 `RequestHeader` `Annotation`은 아래**와 같다.

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestHeader {

	/**
	 * Alias for {@link #name}.
	 */
	@AliasFor("name")
	String value() default "";

	/**
	 * The name of the request header to bind to.
	 * @since 4.2
	 */
	@AliasFor("value")
	String name() default "";

	/**
	 * Whether the header is required.
	 * <p>Defaults to {@code true}, leading to an exception being thrown
	 * if the header is missing in the request. Switch this to
	 * {@code false} if you prefer a {@code null} value if the header is
	 * not present in the request.
	 * <p>Alternatively, provide a {@link #defaultValue}, which implicitly
	 * sets this flag to {@code false}.
	 */
	boolean required() default true;

	/**
	 * The default value to use as a fallback.
	 * <p>Supplying a default value implicitly sets {@link #required} to
	 * {@code false}.
	 */
	String defaultValue() default ValueConstants.DEFAULT_NONE;

}
```
