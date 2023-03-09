---
title: "CORS(Cross-Origin Resource Sharing)란? Spring Tomcat 설정과 함께 간략히!"
last_modified_at: 2021-07-14T09:53
categories:
  - concept
tags: 
  - 'cors' 
  - 'preflight' 
  - '교차 출처 리소스 공유'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)


# CORS

**`CORS(Cross-Origin Resource Sharing)`**란 **추가 `HTTP Header`를 사용**하여, 

**`한 출처`에서 실행 중인 `Web Application`**이 **`다른 출처`의 선택한 `Resource`에 접근할 수 있는 `권한`을 부여하도록 `Browser`에 알려주는 체제**이다.

**`Web Application`**은 **`Resource`가 자신의 출처(`도메인`, `프로토콜`, `포트`)와 다를 때 `Cross-Origin HTTP Request`를 실행**한다.

<br>

**기본적으로 `Browser`는 보안상의 이유**로, 

**Srcipt단에서 `Cross-Origin HTTP Request`를 제한**한다.


> https://ddaja.com (Front-End Server) -> XMLHttpRequest(JS) -> https://api.ddaja.com/resources (X)

**`XMLHttpRequest`와 `Fetch API`**는 **`동일 출처 정책`**을 **기본 정책으로 수행**한다.
_**동일 출처 정책** : **자신의 출처(Web Application)와 동일한 Resource만** 불러 올 수 있음._

만약 **다른 출처의 Resource를 불러오려면 다른 출처 측**에서 **올바른 `CORS Header`를 포함한 `Response`를 반환해야 한다.**

![](https://media.prod.mdn.mozit.cloud/attachments/2016/10/28/14295/a21a85eaccd405d608395b4ca8d82538/CORS_principle.png)
>_[교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)_


# CORS 공유 표준


## Origin Header

**`CORS 공유 표준`**은 **해당 Resource에 접근하는 것이 허용된 출처**를 **새로운 `HTTP Header`를 통해 제공하여 공유**한다.

```
Request(https://ddaja.com)

GET https://api.ddaja.com/resources/1 HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:71.0) Gecko/20100101 Firefox/71.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
// 출처 제공
Origin: https://ddaja.com



Response(https://api.ddaja.com)

HTTP/1.1 200 OK
Date: Mon, 14 07 2021 00:23:53 KST
Server: Apache/2
// 응답 제공하는 서버 측에 설정된 Allow-Origin에 포함되면 공유 가능
Access-Control-Allow-Origin: https://ddaja.com, https://haja.com
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml
```

## Preflight - Options method

**`CORS 명세`** 중 **`Preflight`하여 실제 Request 전송 전에 먼저 허용 가능한지 Request를 전송 하여 `Resource`를 공유하는 명세**가 있다.

<br>


**요청을 받는 다른 출처의 `WebApplication` 측**에서,

**`Resource`에 대해 `Side Effect`가 발생할 수 있는 `HTTP Request Method`(`POST`, `PUT`, `PATCH`, `DELETE`)에 대한 요청**에 대해,

**요청 하는 `Browser`에서 다른 출처의 `WebApplication`에 `OPTIONS` Method**로,

**`Preflight`하여 허용 가능한 `Method 목록`을 먼저 요청**한 후,

**`접근 허가`가 떨어지면 실제 `Resource`에 대한 Request를 보내도록 요구**한다.

이때 **다른 출처 Web Application 측**에서 **인증 정보(Cookie, SessionID, ...)를 함께 전송해야 한다고 명세할 수도** 있다.


![](https://images.velog.io/images/gillog/post/ab528959-b2e4-464b-b95c-bf9a4996f302/image.png)




위 사진은 다음과 같은 과정으로 이루어진다.

1. `Client`는 먼저 **실제 Request인 `POST` Method를 `Access-Control-Request-Method` Header에 설정**,
**`OPTIONS` Method로 먼저 Preflight하여 허용 가능한지 Server에 점검** 받는다.

2. **`Server`**는 **`Client`의 `Origin`**이 **자신이 설정한 `Access-Control-Allow-Origin`에 포함되었는지**, 
**`Access-Control-Request-Method`**가 **자신이 허용한 `Acess-Control-Allow-Methods`에 포함되었는지**,
**`Access-Control-Request-Headers`**가 **자신이 설정한 `Access-Control-Allow-Header`에 포함되었는지 확인**하여,
**결과를 `Client`에 전송**한다.

3. **`Server`로 부터 `200 OK` `Response`**를 받은 **`Client`는 원래 요청하길 원했던 `POST` Method로 Request를 전송**하여 **최종 Resource를 응답**받는다.



# In Java With Tomcat Server

만약 **Java Web Application에서 Tomcat Server로 CORS 설정을 하고 싶다면**,

아래와 같이 **Tomcat의 `web.xml`에 `filter`설정**을 해둔 후,

**`Filter`를 implements하는 `CORSFilter Class`를 아래와 같은 예시로 선언**하면 된다.


```xml
<filter>
  <filter-name>cors</filter-name>
  <filter-class>project.config.filter.SimpleCORSFilter</filter-class>
  <async-supported>true</async-supported>
</filter>
<filter-mapping>
  <filter-name>cors</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

```java
public class SimpleCORSFilter implements Filter {
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {
		HttpServletResponse httpResponse = (HttpServletResponse) response;
        	// 허용할 Method 목록
		httpResponse.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS");
		httpResponse.setHeader("Access-Control-Max-Age", "3600");
        	// 허용할 Header 목록
		httpResponse.setHeader("Access-Control-Allow-Headers", "x-requested-with, content-type, Authorization");
        	// 허용할 다른 출처 도메인 목록
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

**위 `web.xml` 설정과, `SimpleCORSFilter.Class`**는 **해당 서버의 모든 `URL`을 Filter**하여,

**`Response`의 Header에 설정한 `CORS` Header 정보를 삽입**해 준다. 
