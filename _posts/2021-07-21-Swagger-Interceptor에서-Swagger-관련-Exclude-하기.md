---
title: "[Swagger] Interceptor에서 Swagger 관련 Exclude 하기"
last_modified_at: 2021-07-21T07:11
categories: 
  - spring
tags:
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Spring]Spring MVC Interceptor[LKS님의 블로그]](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=exploit_code&logNo=220144535044)

---


본인의 경우 **API Server로 들어오는 모든 요청에서 `JWT` 검증을 수행하는 `Interceptor`를 만들어 사용**하고 있었다.


```java
public class JwtInterceptor extends HandlerInterceptorAdapter {
	private static final Logger LOG = Logger.getLogger(JwtInterceptor.class);
	@Autowired
	private AuthService authService;

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws IOException, JwtException {
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

이때 **해당 서버에서 API 문서 관리용으로 `Swagger`를 사용 중**이었는데,

**해당 검증 과정이 `Swagger` 페이지를 요청할때도 수행**되고 있었다.



이를 위해 **아래처럼 `Interceptor`에서 `HttpServletRequest`의 url를 검증**하여 **`Interceptor`에서 제외할 수도** 있지만,




```java
public class JwtInterceptor extends HandlerInterceptorAdapter {
	private static final Logger LOG = Logger.getLogger(JwtInterceptor.class);
	@Autowired
	private AuthService authService;

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws IOException, JwtException {
		String url = request.getRequestURI();
		if (url.contains("swagger") || url.contains("api-docs") || url.contains("webjars")) {
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


**`Distpatcher Servlet`의 `Context(servlet-context.xml)`안에서 아래와 같이 아예 `Interceptor`의 대상 경로에서 제외**할 수도 있다.

```
	<mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/**" />
			<bean id="loggerInterceptor" class="project.config.logger.LoggerInterceptor"/>
		</mvc:interceptor>
		<mvc:interceptor>
			<mvc:mapping path="/**"/>
			<mvc:exclude-mapping path="/v2/api-docs"/>
			<mvc:exclude-mapping path="/swagger-resources/**"/>
			<mvc:exclude-mapping path="/swagger-ui.html"/>
			<mvc:exclude-mapping path="/webjars/**"/>
			<bean id="jwtInterceptor" class="project.config.interceptor.JwtInterceptor"/>
		</mvc:interceptor>
	</mvc:interceptors>
```

<br>

# 결론

**어떤 방식이 되었던 `/v2/api-docs`, `swagger-ui`, `swagger-resources`, `webjars` 가 포함된 경로들**은,

**`Swagger`에서 사용하고 있는 경로들**이라 **검증을 수행하는 `Interceptor`**라면 **제외시켜주면 정상적으로 `Swagger`가 동작**한다.