---
title: "[Spring] DispatcherServlet의 예외처리 전략(HandlerExceptionResolver)"
last_modified_at: 2021-09-29T04:02
categories: 
  - spring
tags: 
  - '@ExceptionHandler' 
  - 'DispatcherServlet' 
  - 'HandlerExceptionResolver' 
  - 'HandlerInterCeptor' 
  - 'Java' 
  - 'Spring' 
  - 'exception' 
  - 'exception handling' 
  - 'resolveException'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[토비의 스프링 3.1 vol.2 스프링의 기술과 선택](http://www.acornpub.co.kr/book/toby-spring3-1-vol2)


---

```java
public class JwtInterceptor extends HandlerInterceptorAdapter {
	@SuppressWarnings("unused")
	private static final Logger LOG = Logger.getLogger(JwtInterceptor.class);
	@Autowired
	private TokenService tokenService;
	private static final String SWAGGER_URI = "swagger-ui.html";

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws AuthenticationException {
		if (request.getMethod().equals("OPTIONS")) {
			return true;
		}
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			LOG.error("[JwtInterceptor] JWT is null");
			throw new AuthenticationException("JWT is required");
		}
		try {
			ResultJWT resultJWT = tokenService.getResultJWT(jwt);
			if (request.getRequestURI().contains(SWAGGER_URI)
					&& ((resultJWT.getRole() == null) || JWTRoles.USER.getRole().equals(resultJWT.getRole()))) {
				LOG.error("[JwtInterceptor] JWT Role is not Allowed");
				throw new AuthenticationException("JWT Role is not Allowed");
			}
		} catch (JwtException e) {
			LOG.error("[JwtInterceptor] JwtException Throw");
			throw e;
		}
		return true;
	}
}
```

현재 **개발하고 있는 프로젝트단에서 JWT를 활용한 인증이 필요한 Handler**를

**공통적으로 인증 과정을 수행**하는 **`HandlerInterceptorAdapter`를 상속받는 `JwtInterceptor`를 위와 같이 사용**하고 있었다.


위 **인증 과정 중 인증 실패시 아래** 처럼 **Exception을 throw 하는 부분**이 있는데,

```java
throw new AuthenticationException("JWT is required");
```

해당 프로젝트에서 **`@ControllerAdvice`를 통해서 공통으로 Exception을 Handling 하고 있던 상황**이라,

**정상적으로 Exception Handling이 수행되고 있다고 생각**하고 있었다.

```java

@RestControllerAdvice
public class ExceptionController {
	@SuppressWarnings("unused")
	private static final Logger LOG = Logger.getLogger(ExceptionController.class);
	private static final String DEFALUT_MESSAGE_AUTHENTICATION = "Authentication is not valid";

	@Autowired
	private ExceptionService exceptionService;

	@ExceptionHandler(AuthenticationException.class)
	protected ResponseEntity<ErrorResponse> handleAuthenticationException(AuthenticationException e,
			HttpServletRequest request) {
		exceptionService.errorLog(e, request);
		String message = e.getMessage() == null ? DEFALUT_MESSAGE_AUTHENTICATION : e.getMessage();
		return new ResponseEntity<>(new ErrorResponse(HttpStatus.UNAUTHORIZED.value(), message, request.getSession().getId()),
				HttpStatus.UNAUTHORIZED);
	}
}
```

![](https://images.velog.io/images/gillog/post/1ad53e3a-c357-4a8d-8ba5-86dc4772e4e3/image.png)

하지만 **위와 같이 해당 Interceptor 단에서 throw 하던 Exception들**은,

**`@ControllerAdvice`단에서 Handling 하지 못하고** 있었고,


**결국에는 `preHandle()` 단에서 `throw`가 아닌 `return false`로 handler로 전달되지 않는 Logic으로 변경**하였다.




**오늘은 이를 해결하는 동안 파악한 Exception Handling 관련 내용들을 gillog** 한다.

---

# HandlerInterceptor

먼저 **아래 그림을 통해 Spring에서 MVC Request의 Lifecycle 과정을 확인**해보면

>![](https://images.velog.io/images/gillog/post/9b95c94c-018f-4262-a13b-78cc5bae59ef/image.png)
[🙈[Spring] Interceptor (1) - 개념 및 예제🐵[victorydntmd.tistory.com]](https://victorydntmd.tistory.com/176)

**`HandlerInterceptor`단**은 **`Handler(Controller)` 수행 전**에,

**`DispatcherServlet`에게 `Request`를 위임받아 처리하는 객체**이다.

<br>

**`@ControllerAdvice`는 Handler(Controller)단에서 발생하는 Exception**을,

**`@ExceptionHandler` Annotation으로 처리하는 Logic**이라 **Exception Handling이 불가능한 상황**이었다.

<br>


**원래 의도했던 Logic**이 **해당 Handler로 접근하기 전 올바른 권한을 확인** 하고,

**정상 권한이 없는 상황**에서 **접근 제한 용도로 작성했던 `throw AuthenticationException()` Logic**이라,

단순히 **`preHandle()`에서 `return false`를 통해 해당 Handler로 접근을 제한하는 방법으로 변경**했다.

<br>

**아래 내용 부터**는 **Spring에서 `Exception Handling`이 처리되는 과정**을,

**이번 기회를 통해 알게된 사항들을 작성한 부분**이다.

---

# HandlerInterceptor단 Exception Handling

**`Handler(Controller)`에서 발생한 Exception**을 **Handling 하기 위해서**는,

**`HandlerExceptionResolver`의 Exception Handling 전략을 사용**해야 한다.


## HandlerExceptionResolver 활용

**`HandlerExceptionResolver`**는 **Controller의 작업 중 발생한 Exception을 처리하는 결정 전략 객체**이다.


**`Handler(Controller)`나 그 뒤 계층에서 throw된 Exception**은,

**`DispatcherServlet`이 일단 전달 받은 뒤**,

다시 **Servlet 밖의 Servlet Container가 처리**하게 되는데,

**`HandlerExcetpionResolver`가 등록되어 있지 않다면**,

**`HTTP Status 500 내부 서버 오류` 와 같은 메시지가 브라우저 단에 노출**된다.

<br>

하지만 **`HandlerExceptionResolver`가 등록되어 있는 경우**,

**`DispatcherServlet`**은 먼저 **`HandlerExceptionResolver`가 해당 Exception을 Handling 할 수 있는지 확인**하고,

**Exception Handling이 가능**한 경우 **해당 `HandlerExceptionResolver`에게 Exception Handling을 위임**한다.


<br>

**`HandlerExceptionResolver`를 구현**하기 위해서는 **아래 Interface를 implements**하면 되는데,

```java
public interface HandlerExceptionResolver {

	/**
	 * Try to resolve the given exception that got thrown during handler execution,
	 * returning a {@link ModelAndView} that represents a specific error page if appropriate.
	 * <p>The returned {@code ModelAndView} may be {@linkplain ModelAndView#isEmpty() empty}
	 * to indicate that the exception has been resolved successfully but that no view
	 * should be rendered, for instance by setting a status code.
	 * @param request current HTTP request
	 * @param response current HTTP response
	 * @param handler the executed handler, or {@code null} if none chosen at the
	 * time of the exception (for example, if multipart resolution failed)
	 * @param ex the exception that got thrown during handler execution
	 * @return a corresponding {@code ModelAndView} to forward to,
	 * or {@code null} for default processing in the resolution chain
	 */
	ModelAndView resolveException(
			HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex);

}
```

<br>

**구현해야 하는 `resolveException()` method는 return type이 `ModelAndView`**로,

**예외 노출에 사용될 View**와 **그 안에 포함될 내용을 담은 Model 객체를 return 하도록 작성**해주면 된다.





<br>

**`@ExceptionHandler`를 통해서 Exception을 Handling 하는 전략**도,

**`AnnotationMethodHandlerExcetpionResolver` 방식**으로,

**네 가지 `HandlerExceptionResolver` 구현 전략 중 한 가지** 이다.
_**`ResponseStatusExceptionResolver`** : **Exception을 특정 HTTP 응답 상태 코드로 전환 해주는 전략**
**`DefaultHandlerExceptionResolver`** : **`404(NoSuchRequestHandlingMethodException)`, `400(TypeMissmatchException)`** 등 기본 **Spring에서 마지막으로 Exception을 Handling 해주는 표준 예외처리 전략**
**`SimpleMappingExceptionResolver`** : **`web.xml`의 `<error-page>`등으로 선언하는 예외 처리 view 지정 전략**_


---

그렇다면 **`HandlerInterceptor`단에서 Exception을 어떻게 처리**할 수 있을까

## Request 객체 forward

**그나마 접근한 방법**은 **`HandlerInterceptor`단에서 `preHandle()` method**에서,

**해당 `Request`가 가고자하는 handler가 아닌**,

**Exception 처리를 관장하는 Handler로 Request를 Forward 시키는 방법**이다.

<br>


아래 코드를 살펴보자.

```java
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws AuthenticationException {
		if (request.getMethod().equals("OPTIONS")) {
			return true;
		}
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			LOG.error("[JwtInterceptor] JWT is null");
            
			request.setAttribute("message", "JWT is required");
            		request.setAttribute("exception", "AuthenticationException");
            		request.getRequestDispatcher("/error").forward(request, response);
            
		}
		return true;
	}
            
```

위 코드와 같이 **`HandlerInterceptor`단에서 예외상황 발생**시,

**`/error`와 같은 `Handler(Controller)`단으로 request를 `forward`** 시킨 후,

```java
    @GetMapping("/error")
    @ResponseBody
    public void handlerError(HttpServletRequest request) throws Exception {
        String message = (String) request.getAttribute("message");
        String exception = (String) request.getAttribute("exception");
 
        if(exception.equals("AuthenticationException")){
            throw new AuthenticationException(message);
        } else {
            throw new Exception(message);
        }
    }
```

**`Handler(Controller)`를 등록**하여 **`@ControllerAdvice`단에서 Exception을 Handling 할 수 있도록 처리하는 방법**이다.



**위 방법으로 처리하는것 보다**,

**예외 상황 발생 시 Interceptor 단에서 그냥 Handler로의 접근을 `return false`로 끊어버리는게 더 올바른 방법이라 생각**하여,

<br>

**해당 방법은 실제로 사용하지는 않았다.**

<br>

**만약 비슷한 고민을 가지고 이 글을 읽고 계신 분**이 계시다면,

**더 현명한 처리 전략에 대해 고민 하실 수 있기를 바라며 gillog를 마친다.** 🙋‍♂️