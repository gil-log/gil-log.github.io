---
title: "[Spring] Interceptor란? 구현 예제와 함께 (HandlerInterceptor, HandlerInterceptorAdapter)"
last_modified_at: 2021-07-21T02:04
categories: 
  - spring
tags: 
  - 'HandlerInterceptorAdapter' 
  - 'Interceptor' 
  - 'Spring'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Spring Framework] HandlerInterceptorAdapter[jhkang-dev]](https://jhkang-tech.tistory.com/53)

[[spring] Spring Interceptor 란?(HandlerInterceptor, HandlerInterceptorAdapter)[Kim VamPa]](https://kimvampa.tistory.com/127)

[]()


---

# Interceptor

**`Interceptor`**는 **`낚아채다`의 의미**를 가지고있다.


**`Client`에서 `Server`로 들어온 `Request` 객체**를,

**`Controller`의 `Handler`로 도달하기 전 가로채**어,

**원하는 추가 작업이나 로직을 수행 한 후 `Handler`로 보낼 수 있도록 해주는 `Module`**이다.
_**`Handler` : 사용자가 요청한 url에 따라 실행되어야 할 Method**_


<br>

**아래 그림을 통해 Spring 단에서 살펴보면**,

![](https://images.velog.io/images/gillog/post/d835930e-5718-43a3-b19d-4e111275a54a/image.png)



**`DispatcherServlet`**은 **`HandlerMapping`에게 Client `Request`를 수행 할 `Handler`를 찾도록 요청**을 보낸다.

**이때 `HandlerExecutionChain(핸들러 실행 체인)`이 동작**하는데,

이 **`HandlerExecutionChain`**은 **하나 이상의 `HandlerInterceptor`를 거쳐 `Controller`가 실행되도록 구성**되어 있다.
_**`HandlerInterceptor`를 등록하지 않았을 경우 바로 `Controller` 실행**_




**`HandlerInterceptor`를 거쳐 `Request`에 대해 원하는 `작업`, `로직`을 수행**한 후 **`Controller`로 `Request` 객체를 전달**한다.
_Login Session 검증, Header 검증, Token 검증, URL Handling 등등..._


## Why ??

**`Interceptor`**는 **`Controller`의 `Handler`가 실행되기 전이나 후**에 **`추가적인 작업`이 수행되어야 할때 사용**한다.

이때 **`Interceptor`를 활용하여 얻을 수 있는 장점은 크게 아래 세 가지**와 같다.


- **공통 코드 사용으로 코드 재사용성 증가**

- **메모리 낭비, 서버 부하 감소**

- **코드 누락에 대한 위험성 감소**

각 장점을 아래 예시 코드와 함께 살펴보자.


```java
@AllArgsConstructor
@RequestMapping("admin")
@RestController
public class AdminController {

    private AdminService adminService;

    @PostMapping("/login")
    public ResponseEntity<CommonResponse> loginAdmin(@RequestHeader(value = "Authorization") String jwt, @RequestBody UserDTO userDTO) throws JwtException {
    	...jwt 검증 및 로그인 검사..
        return ResponseEntity.ok().body(new CommonResponse());
    }
}
```

만약 위와 같이 **`Request Header`에서 `JWT`를 받아**와 **검증 실패시 `JwtException`을 `throw`해 `JWT` 검증을 수행**하고, 

**검증 성공시 `Request Body`의 정보로 관리자(admin)의 login을 수행하는 `Handler`가 있다고 가정**하자.

**초기 개발단계에서는 위와 같은 `Handler` 하나 뿐**이었지만, **만약 `/admin`에 대한 모든 Handler에서 JWT 검증을 수행**해야 한다면 어떨까

```java

@AllArgsConstructor
@RequestMapping("admin")
@RestController
public class AdminController {

    private AdminService adminService;

    @PostMapping("/login")
    public ResponseEntity<CommonResponse> loginAdmin(@RequestHeader(value = "Authorization") String jwt, @RequestBody UserDTO userDTO) throws JwtException {
    	...jwt 검증 및 로그인 검사..
        return ResponseEntity.ok().body(new CommonResponse());
    }
    
    @PostMapping("/register")
    public ResponseEntity<CommonResponse> registerAdmin(@RequestHeader(value = "Authorization") String jwt, @RequestBody UserDTO userDTO) throws JwtException {
    	...jwt 검증 및 회원가입 수행..
        return ResponseEntity.ok().body(new CommonResponse());
    }
 
    ....
 
     @GetMapping("/{id}")
    public ResponseEntity<CommonResponse> detailAdmin(@PathVariable(name="id") String id){
    	...jwt 검증 및 세부 정보 ..
        return ResponseEntity.ok().body(new CommonResponse());
    }
}
```

**모든 Handler에서 JWT 검증을 수행하며 JwtException을 Throw 하는 것 로직이 계속해서 늘어나게된다.**
_**모든 Handler에서 같은 로직을 수행하는 코드가 존재해 메모리, 서버 부하 증가**_

또한 **`detailAdmin`와 같이 특정 `Handler`에서 검증 로직이 누락되어 개발될 가능성이 존재**한다.
_**검증 로직 누락으로 인해 위험성 증가**_


**위와 같은 상황**에서 **`Interceptor`를 활용하면 JWT 검증 로직을 `Interceptor`에서 한번만 작성**하고,

**`/admin/*`에 대한 요청에 `Interceptor`를 적용**하여 **작성 코드량을 줄여 메모리 낭비를 줄일 수** 있고,

**`Interceptor` 적용의 기준 `url`을 명시**해 줌으로써 **일괄적으로 `/admin/*`에 해당하는 `Handler`들에 적용**해주어 **코드 누락의 위험성을 줄일 수** 있다.


---

# Interceptor 구현하기

**`Spring`에서 `Interceptor`의 구현**은 **`HandlerInterceptor(Interface)`나 `HandlerInterceptorAdapter(Abstract Class)`로 구현**할 수 있다.

_**`HandlerInterceptorAdapter` Abstract Class는 `HandlerInterceptor` Interface를 상속받아 구현**됨_

**위 두가지 중 하나를 구현하는 Class를 생성**한 후, 

**`DispatcherServlet`의 Context(servlet-context.xml)에 작성 `Interceptor` Class를 Bean 등록**하고,

**적용 Url을 명시**해주면 된다.

## 구현 메소드

**`Interceptor` 구현을 위해 아래와 같은 네 가지 Method**가 있다.



### PreHandle(HttpServletRequest request, HttpServletResponse response, Object handler)

**`Controller(RequestMapping 선언 Handler)` 실행 직전에 동작하는 method.**

**`return` 값이 true일 경우 정상적으로 진행**이 되고, 

**false일 경우 실행 종료**
_**`Controller` 진입 X**_

**Parameter 중 `Object handler`**는 **`HandlerMapping`이 찾은 Controller Class 객체**이다.

<br>

### PostHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView)

**`Controller` 진입 후 `View`가 `Rendering` 되기 전 수행**된다.

Parameter 중 **`ModelAndView modelAndView`를 통해 화면 단에 들어가는 Data 등의 조작이 가능**하다.

<br>


### afterComplete(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)

**`Controller` 진입 후 `View`가 정상적으로 `Rendering` 된 후 수행**된다.


<br>


### afterConcurrentHandlingStarted(HttpServletRequest request, HttpServletResponse response, Object handler)

**`Servlet 3.0`부터 `비동기 요청`이 가능해지며 생겨난 method**로,

**`비동기 요청` 시 `PostHandle`와 `afterCompletion` method를 수행하지 않고 이 메서드를 수행**한다.

---

## 구현 예제

1. **`Maven`을 기준으로 `pom.xml`에 `spring-web` Dependency가 존재하는지 확인**한다.

```
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

2. **`HandlerInterceptor`나 `HandlerInterceptorAdapter`를 구현하는 자신만의 Interceptor Class를 구현**한다.

```java
// HandlerInterceptor 상속
public class CustomInterceptor implements HandlerInterceptor
{
 
}
 
// HandlerInterceptorAdaptor 상속
public class CustomInterceptor extends HandlerInterceptorAdapter
{
 
}
```


```java
public class JwtInterceptor extends HandlerInterceptorAdapter {
	private static final Logger LOG = Logger.getLogger(JwtInterceptor.class);
	@Autowired
	private AuthService authService;

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws IOException, JwtException {
		LOG.info("jwt interceptor cateched!!!");
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			LOG.error("JWT is null");
			throw new AuthenticationException("JWT is null");
		}
		try {
			authService.vertifyJwt(jwt);
		} catch (JwtException e) {
			LOG.error(jwt);
			LOG.error("JWT is not valid");
			throw new AuthenticationException("JWT is not valid");
		}
		return true;
	}
}
```

3. **`DistpatcherServlet`의 Context 파일(servlet-context.xml)에 Interceptor Class와 적용 URL을 명시**한다.

```
	<mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/**"/>
			<bean id="jwtInterceptor" class="project.config.interceptor.JwtInterceptor"/>
		</mvc:interceptor>
	</mvc:interceptors>
```

만약 **`Interceptor`에서 제외하고 싶은 경로가 있을경우 `<mvc:exclude-mapping path=""/>`를 활용**한다.

```
	<mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/**"/>
                        <mvc:exclude-mapping path="/swagger"/>
			<bean id="jwtInterceptor" class="project.config.interceptor.JwtInterceptor"/>
		</mvc:interceptor>
	</mvc:interceptors>
```



![](https://images.velog.io/images/gillog/post/39b4b64c-576c-4e76-9577-08c7186e1809/image.png)

**위 단순 세 과정 만으로 `Spring`에서 `Interceptor`를 활용**할 수 있다.
