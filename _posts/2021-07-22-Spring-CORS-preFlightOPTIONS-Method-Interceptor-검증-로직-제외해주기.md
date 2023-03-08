---
title: "[Spring] CORS preFlight(OPTIONS Method) Interceptor 검증 로직 제외해주기(Access to XMLHttpRequest at '' from origin '' has been blocked by CORS policy)"
last_modified_at: 2021-07-22T04:43
categories: 
  - 에러
tags: 
  - 'CORS policy' 
  - 'Interceptor' 
  - 'Spring' 
  - 'cors' 
  - 'options' 
  - 'preflight'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
이 글을 남기는 이유는,

**JWT 검증을 수행**하는 **`Interceptor`에 의해**서 **다른 출처(다른 서버)에서의 XHR Request**의 **`OPTIONS`로 Request 허용 여부를 먼저 확인**하는 **`preFilght` Request**가 **충돌하는 상황을 길록하기 위해서**다.


# 문제 상황

나의 상황은 아래와 같았다.


먼저 **`CORS` 정책을 제공하는 `CORSFilter`**를 **`Tomcat Server` Configure 인 `web.xml`**에,




![](https://images.velog.io/images/gillog/post/3e10210a-713c-4d29-8c69-7a667144ea35/image.png)


```java
public class SimpleCORSFilter implements Filter {
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		HttpServletResponse httpResponse = (HttpServletResponse) response;
		httpResponse.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS");
		httpResponse.setHeader("Access-Control-Max-Age", "3600");
		httpResponse.setHeader("Access-Control-Allow-Headers", "x-requested-with, content-type, Authorization");
		httpResponse.setHeader("Access-Control-Allow-Origin", "*");
		chain.doFilter(request, response);
	}

	@Override
	public void init(FilterConfig filterConfig) {
		// Do nothing
	}

	@Override
	public void destroy() {
		// Do nothing
	}
}
```

**JWT 검증을 수행하는 `Interceptor`**를 **`DistpatcherServlet`의 Configure인 `servlet-context.xml`에 등록**해두었다.


![](https://images.velog.io/images/gillog/post/0b579c13-24f7-4772-9676-1d2df73832d8/image.png)


```java
public class JwtInterceptor extends HandlerInterceptorAdapter {
	private static final Logger LOG = Logger.getLogger(JwtInterceptor.class);
	@Autowired
	private AuthService authService;

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception, JwtException {
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			throw new AuthenticationException("JWT is null");
		}
		try {
			authService.vertifyJwt(jwt);
		} catch (JwtException e) {
			LOG.error("[JwtInterceptor] JwtException Throw");
			throw e;
		}
		return true;
	}
}
```


<br>


그 후 **동일 출처인(Swagger UI)에서 API 테스트를 수행**하다.

**다른 서버에서 최종 테스트**를 해보려는데,

아래와 같이 **`CORS Policy` 관련 `XHR Request` 실패 메시지가 발생**했다.

```js
Access to XMLHttpRequest at 'https://api.gillog.com:87/notices' 
from origin 'https://gillog.com:84' 
has been blocked by CORS policy: 
Response to preflight request doesn't pass access control check: 
It does not have HTTP ok status.
```

나는 최초에 **`CORS Filter`가 `Interceptor`가 구동되는 시점인 `DispatcherServlet`으로 Request가 오기전 Server 단에서 처리**되어,

**문제가 발생하지 않을거라 생각**했다.

# 문제 원인


**`CORS Filter`**에서는 아래의 **`Authorization` Header를 Allow 해준다는 Response를 Header에 지정**해준다.

```java
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		HttpServletResponse httpResponse = (HttpServletResponse) response;
		httpResponse.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS");
		httpResponse.setHeader("Access-Control-Max-Age", "3600");
                // Authorization Allow
		httpResponse.setHeader("Access-Control-Allow-Headers", "x-requested-with, content-type, Authorization");
		httpResponse.setHeader("Access-Control-Allow-Origin", "*");
		chain.doFilter(request, response);
```

**`Interceptor`는 `Request`의 `Header`에 있는 `Authorization`의 값을 통해 검증을 수행**한다.


```java
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception, JwtException {
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			throw new AuthenticationException("JWT is null");
		}
		try {
			authService.vertifyJwt(jwt);
		} catch (JwtException e) {
			LOG.error("[JwtInterceptor] JwtException Throw");
			throw e;
		}
		return true;
	}
```


결론부터 말하자면, **다른 출처의 자원 공유 정책(CORS)에서 실제 `Request`를 전송 전**에,

**`preFlight`로 `OPTIONS` Http Method로 Reqeust가 허용 가능한지 확인**을 수행하는데,

이 **`preFlight` `Request`** 역시 **`CORS Filter`**의 **`.doFilter()` 후에 `Interceptor`의 검증 로직에** 걸리게 되고,

이때 **`Authorization`**은 **`preFlight` `Request`에 아직 허용되지 않은 `Header`**여서 **전송될 수 없었고**,

**`preFlight` `Request`가 실패**하여 **최종 `XHR Requeset`가 Fail 되었던 것**이다.

**그림을 통해 보면 아래와 같은 상황**이었다.

![](https://images.velog.io/images/gillog/post/9b8ca84d-92b9-43a3-9aac-9c2a1f2b97e7/image.png)

# 문제 해결


**JWT 검증을 수행하는 `Interceptor`**에 **`Request`의 `Method`가 `preFlight` 용 `OPTIONS`일 경우**,

**검증을 수행하지 않는 로직을 추가**해주었다.


```java
public class JwtInterceptor extends HandlerInterceptorAdapter {
	private static final Logger LOG = Logger.getLogger(JwtInterceptor.class);
	@Autowired
	private AuthService authService;

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception, JwtException {
                // Request의 Method가 OPTIONS 인 경우 통과
		if (request.getMethod().equals("OPTIONS")) {
			return true;
		}
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			throw new AuthenticationException("JWT is null");
		}
		try {
			authService.vertifyJwt(jwt);
		} catch (JwtException e) {
			LOG.error("[JwtInterceptor] JwtException Throw");
			throw e;
		}
		return true;
	}
}
```

**이를 통해 문제를 해결**할 수 있었다.

**오늘도 해결 완료! :) 🙆‍♀️**