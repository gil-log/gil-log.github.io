---
title: "[Spring] 404 Error Custom하기 (@ExceptionHandler사용, NoHandlerFoundException Throw 안될때)"
last_modified_at: 2021-07-21T06:52
categories: 
  - spring
tags: 
  - '404' 
  - '@ExceptionHandler' 
  - 'NoHandlerFoundException' 
  - 'Spring' 
  - 'controlleradvice'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[HOWTO handle 404 exceptions globally using Spring MVC configured using Java based Annotations
](https://stackoverflow.com/questions/24498662/howto-handle-404-exceptions-globally-using-spring-mvc-configured-using-java-base)


[[java] 스프링 부트 REST 서비스 예외 처리[리뷰나라]](http://daplus.net/java-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-rest-%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC/)

---

`Spring`에서 아래와 같이 **`ControllerAdvice`로 `@ExceptionHandler`를 사용해 Error들을 Handling** 할 수 있는데,
_@RestControllerAdvice는 @ResponseBody가 붙은 @ControllerAdvice_

```java
@RestControllerAdvice
public class ExceptionController {
	@SuppressWarnings("unused")
	private static final Logger LOG = Logger.getLogger(ExceptionController.class);
	@Autowired
	private ExceptionService exceptionService;

	@ExceptionHandler(HttpRequestMethodNotSupportedException.class)
	protected ResponseEntity<ErrorResponse> handleHttpRequestMethodNotSupportedException(
			HttpRequestMethodNotSupportedException e, HttpServletRequest request) {
		exceptionService.errorLog(e, request);
		return new ResponseEntity<>(
				new ErrorResponse(HttpStatus.METHOD_NOT_ALLOWED.value(), "MethodNotSupported", e.getMessage()),
				HttpStatus.METHOD_NOT_ALLOWED);
	}

	@ExceptionHandler(NoHandlerFoundException.class)
	protected ResponseEntity<ErrorResponse> handleNoHandlerFoundException(NoHandlerFoundException e,
			HttpServletRequest request) {
		exceptionService.errorLog(e, request);
		return new ResponseEntity<>(new ErrorResponse(HttpStatus.NOT_FOUND.value(), "Not Found", e.getMessage()),
				HttpStatus.NOT_FOUND);
	}

	@ExceptionHandler(Exception.class)
	protected ResponseEntity<ErrorResponse> handleException(Exception e, HttpServletRequest request) {
		exceptionService.errorLog(e, request);
		return new ResponseEntity<>(new ErrorResponse(HttpStatus.BAD_REQUEST.value(), request.getSession().getId()),
				HttpStatus.BAD_REQUEST);
	}

	@ExceptionHandler(JwtException.class)
	protected ResponseEntity<ErrorResponse> handleJwtException(JwtException e, HttpServletRequest request) {
		exceptionService.errorLog(e, request);
		return new ResponseEntity<>(new ErrorResponse(HttpStatus.UNAUTHORIZED.value(), "JwtException", e.getMessage()),
				HttpStatus.UNAUTHORIZED);
	}

	@ExceptionHandler(AuthenticationException.class)
	protected ResponseEntity<ErrorResponse> handleAuthenticationException(AuthenticationException e,
			HttpServletRequest request) {
		exceptionService.errorLog(e, request);
		return new ResponseEntity<>(
				new ErrorResponse(HttpStatus.UNAUTHORIZED.value(), "AuthenticationException", e.getMessage()),
				HttpStatus.UNAUTHORIZED);
	}
}
```

**`404 Error`을 Handling하기 위해 `NoHandlerFoundException`을 `@ExceptionHandler`로 Handling** 해보려 했지만 **적용되지 않았다.**

<br>

**이를 해결하기 위해**선 **`DispatcherServlet`이 `HandlerMapping`에게 `Request`를 처리할 `Handler`를 찾도록 요청할 때**,

**`Handler`를 찾지 못할 경우 `NoHandlerFoundException`을 Throw 하게 해주어야** 한다.


<br>

**방법은 서버 환경에 따라 다른데, 자신의 서버 환경에 따라 아래 방법을 설정**하자.

# Spring


## Spring - Tomcat web.xml

```
	<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/config/servlet-context.xml</param-value>
		</init-param>
		<init-param>
			<param-name>throwExceptionIfNoHandlerFound</param-name>
			<param-value>true</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
		<async-supported>true</async-supported>
	</servlet>
```

---

# Spring Boot

## Spring Boot - extends SpringBootServletInitializer

```java
@ComponentScan()
@EnableAutoConfiguration
public class MyApplication extends SpringBootServletInitializer {
    public static void main(String[] args) {
        ApplicationContext ctx = SpringApplication.run(MyApplication.class, args);
        DispatcherServlet dispatcherServlet = (DispatcherServlet)ctx.getBean("dispatcherServlet");
        dispatcherServlet.setThrowExceptionIfNoHandlerFound(true);
    }
}
```

## Spring Boot - application.properties

```xml
spring.mvc.throw-exception-if-no-handler-found=true
spring.resources.add-mappings=false
```

## Spring Boot - *.yml

```yml
spring:
# error 404
mvc:
  throw-exception-if-no-handler-found: true
  dispatch-options-request: false
```

_21-08-28 추가 위 방법 말고 아래 방법으로 성공했다는 의견 존재_

```yml
spring:
web:
    resources:
        add-mappings: false 
```



**위 설정 이후**에 **정상적으로 `NoHandlerFoundException`이 Throw**되어,

**`@ExceptionHandler(NoHandlerFoundException.class)`에서 Handling 할 수 있었다.**

![](https://images.velog.io/images/gillog/post/76e25edc-dec5-47a2-a56d-dce7fb5774b2/image.png)

**오늘도 해결 완료 :) 🙆‍♀️**