---
title: "REST API"
last_modified_at: 2021-05-26T00:39
categories: 
  - 개념
tags: 
  - 'API' 
  - 'REST' 
  - 'restful api'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[REST API 제대로 알고 사용하기[NHN Cloud Meetup!]](https://meetup.toast.com/posts/92)

[RESTful API 설계 가이드[학학이]](https://sanghaklee.tistory.com/m/57)

[REST 아키텍처를 훌륭하게 적용하기 위한 몇 가지 디자인 팁[spoqa.github.io]](https://spoqa.github.io/2012/02/27/rest-introduction.html)
<br>

---

# REST API

**`REST API`에서 `REST`**는 **`Representational State Transfer`라는 용어의 약자**이다. 

**`REST`는 2000년도 로이 필딩(Roy Fielding)이라는 HTTP의 주요 저자 박사학위 논문에서 최초 소개**되었고, 

**웹(HTTP) 설계의 우수성에 비해 제대로 사용되어지지 못하는 모습에 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 `REST`를 발표**했다.


## REST API의 구성

**REST API는 `자원(Resource)`, `행위(Verb)`, `표현(Representations)` 이렇게 세 가지로 구성**되어 있다.

여기서 **`자원(Resource)`은 URI의 형태로, `행위(Verb)`는 HTTP의 Method로 사용**한다.


## REST API의 특징

**`REST API`는 `Uniform Interface`, `Stateless`, `Cacheable`, `Self-Descriptiveness`, `Client-Server 구조`, `계층형 구조`의 6가지 특징**을 지닌다.

### Uniform Interface

**`Uniform Interface`는 URI로 지정한 리소스에 대한 조작을 통일**되고 **한정적인 Interface로 수행하는 아키텍처 스타일**을 말한다.

### Stateless

**`REST API`는 무상태성 성격**을 갖는다. 

즉, **작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. **

**세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문**에 **API 서버는 들어오는 요청만을 단순히 처리만**하면 된다. 

이를 통해 **서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음**으로써 **구현이 단순**해진다.

### Cacheable

**`REST API`의 가장 큰 특징 중 하나는 HTTP라는 기존 웹 표준을 그대로 사용**하기 때문에, 
**웹에서 사용하는 기존 인프라를 그대로 활용 가능**하다. 

**따라서 HTTP가 가진 캐싱 기능이 적용 가능**하다. 
_**HTTP 프로토콜 표준에서 사용**하는 **`Last-Modified Tag`나 `E-Tag`를 이용하면 캐싱 구현이 가능**_

### Self-descriptiveness
**`Self-descriptiveness`는 `REST API`의 다른 큰 특징**으로 **`REST API Message`만 보고도 이를 쉽게 이해 할 수 있는 `자체 표현 구조(Self-descriptiveness)`**로 되어 있다.

### Client-Server 구조

**`REST Server`는 REST API를 제공**, **Client는 사용자 인증**이나 **컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조**이다.

**Server와 Client의 역할이 확실히 구분되기 때문**에 **개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 된다.**

### 계층형 구조

**`REST API Server`는 다중 계층으로 구성될 수 있고 `보안`, `로드 밸런싱`, `암호화 계층`을 추가해 구조상의 유연성**을 둘 수 있고 **`PROXY`, `게이트웨이` 같은 네트워크 기반의 중간매체를 사용할 수 있다.**