---
title: "[JSP] EL Implicit Objects(EL 내장 객체)"
last_modified_at: 2021-05-14T00:24
categories: 
  - jsp
tags: 
  - 'EL' 
  - 'Implicit Objects' 
  - 'applicationScope' 
  - 'cookie' 
  - 'header' 
  - 'headerValues' 
  - 'initParam' 
  - 'jsp' 
  - 'pageContext' 
  - 'pageScope' 
  - 'param' 
  - 'paramValues' 
  - 'requestScope' 
  - 'sessionScope'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[WebLogic JSP Reference[Oracle]](https://docs.oracle.com/cd/E12839_01/web.1111/e13712/reference.htm#WBAPP427)

[]()

[]()

[]()

[]()

[]()

<br>


# EL Implicit Objects

`JSP`에서 사용되는 **`JSP EL(Expression Language)`에는 사용할 수 있는 몇 가지 내장 객체들**이 있다. 

이러한 **객체들은 아래 예시처럼 `${???}`형식으로 사용**할 수 있다.

``` JSP
${pageContext.request.requestURI}
// 요청 URL, HttpServletRequest를 호출하여 가져온다.

${sessionScope.gillog}
// session-scoped의 attribute 중 gillog이라는 이름의 Value
// 발견되지 않으면 null

${param.gillog}
// gillog라는 parameter의 String Value를 return
// 발견되지 않으면 null

${paramValues.gillogs}
// gillogs라는 parameter로 포함된 String[] Value를 return
// 발견되지 않으면 null
```

## pageContext

**`pageContext` object**를 가져온다. 


## pageScope

**`page-scoped` attribute의 이름**을 **Value에 Mapping 하는 `Map`**을 가져온다. 

## requestScope

**`request-scoped` attribute의 이름**을 **Value에 Mapping 하는 `Map`**을 가져온다. 


## sessionScope

**`session-scoped` attribute의 이름**을 **Value에 Mapping 하는 `Map`**을 가져온다. 


## applicationScope

**`application-scoped` attribute의 이름**을 **Value에 Mapping 하는 `Map`**을 가져온다. 

## param

**매개 변수 이름**을 **단일 String 매개 변수 값에 Mapping하는 `Map`**을 나타낸다.
_`ServletRequest.getParameter(String name)`을 호출하여 가져온다._

## paramValues

**매개 변수 이름**을 **String 배열 매개 변수 값에 Mapping하는 `Map`**을 가져온다. 
_`ServletRequest.getParameterValues(String name)`을 호출하여 가져온다._

## header

**헤더 이름**을 **단일 String 헤더 값에 Mapping하는 `Map`**을 가져온다. 
_`ServletRequest.getHeader(String name)`을 호출하여 가져온다._


## headerValues

**헤더 이름**을 **String 배열 헤더 값에 Mapping하는 `Map`**을 가져온다. 
_`ServletRequest.getHeaders(String name)`을 호출하여 가져온다._


## cookie

**쿠키 이름**을 **단일 쿠키 개체에 Mapping하는 `Map`**을 가져온다. 
_`HttpServletRequest.getCookies()`를 호출하여 가져온다._ 

동일한 이름을 여러 쿠키에서 공유하는 경우 구현은 getCookies() 메서드에 의해 반환된 쿠키 개체 배열에서 첫 번째 개체를 사용해야 한다. 

반환되는 쿠키 순서는 현재 Servlet 규격에 맞게 지정되어 반환되지 않는다.

## initParam

**context 초기화 parameter name**을 **문자열 매개 변수 값에 Mapping하는 `Map`**을 가져온다.
_`ServletRequest.getInitParameter(String name)`를 호출하여 가져온다._
