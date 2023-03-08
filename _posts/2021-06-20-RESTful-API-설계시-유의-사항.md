---
title: "RESTful API 설계시 유의 사항"
last_modified_at: 2021-06-20T23:42
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

먼저 REST API가 무엇인지 아직 대략적으로 파악이 안된 경우 [여기](https://velog.io/@gillog/RESTful-API) 를 살펴보자.


# REST API 설계시 유의 사항

## Core Rules

### URI는 Information의 Resource를 표현한다.


### /는 계층 관계를 나타내는데 사용한다.

```python
// houses Collection에 있는 apartments Collection에 있는 banpojayi id 21번 Element에 접근
GET /houses/apartments/banpojayi/21
```

### Resource간의 연관 관계는 사이에 /와 Sub Resource로 표현한다.

**Resource간에 연관 관계는 복잡한 관계가 아닐 경우 아래와 같이 표현**한다.

`/resoure1/{:id}/resourse2`

**일반적으로 위와 같은 표현은 `소유(has)` 관계를 표현**한다.

```python
// 111번 유저가 소유한 기기목록 정보 요청
GET /users/111/devices
```

만약 **관계가 복잡한 경우 `Sub Resource`를 통해 표현**할 수 있다.

```python
// 111번 유저가 좋아하는 소유한 기기 목록 정보
GET /users/111/likes/devices
```


### Resource에 대한 행위는 HTTP Method로 표현한다.

```
1번 사용자의 정보 조회 REST API

// X
GET /users/show/1

// O
GET /users/1
```

여기서 **`Resource`는 크게 `Collection`과 `Element`로 나누어 표현**할 수 있다.

|Resource|GET(:id)|POST|PUT(:id)|PATCH(:id)|DELETE(:id)|
|:--:|:--:|:--:|:--:|:--:|:--:|
|Collection<br>`http://gillog.com/collection/`|Collection에 속한 Element들의 URI나 목록을 출력|해당 Collection에 속하는 새로운 Element를 생성|전체 Collection을 수정|전체 Collection의 일부 속성을 수정|전체 Collection을 삭제|
|Element<br>`http://gillog.com/collection/element1`|요청 Collection내 Element를 출력|요청 Element에 귀속되는 새로운 Resource를 생성|전체 element를 수정|전체 Element의 일부 속성을 수정|해당 Collection내 Element를 삭제|

여기서 **`GET`, `PUT`, `PATCH`, `DELETE`는 각각 Resource의 `id` 입력 여부에 따라 기능이 조금씩 달라진다.**


|Method|GET|POST|PUT|PATCH|DELETE|
|:--:|:--:|:--:|:--:|:--:|:--:|
|/users|사용자 전체 조회|사용자 생성|사용자 전체 수정|사용자 전체에 일부 속성 수정|사용자 전체 삭제|
|/users/gillog|사용자 `gillog` 상세 조회|405 ERROR|사용자 `gillog` 수정|사용자 `gillog` 일부 속성 수정|사용자 `gillog` 삭제|
_HTTP State Code 405 : 서버에서 허용되지 않는 방법_

## Other Rules

### Method는 URL에 포함하지 않는다.

```python
//X
POST /users/1/update-posts/1

//O
PATCH /users/1/posts/1
```

### URL 마지막엔 /를 포함하지 않는다.

```python
// X
/users/

// O
/users
```


### 파일확장자는 URI에 포함시키지 않는다.

**`Accept Header`를 사용하여 확장자를 표시**한다.

```python
GET /users/111/profile/photo

HTTP/1.1 200 OK
Accept: image/png
```

### _대신 -를 사용한다.

**`-`의 사용도 최소화 하며 설계**해야 한다.

**`-`를 사용하는 경우**는 **정확한 의미 전달이나 표현을 위해 단어 결합이 불가피한 경우에 사용**한다.

```python
// X
/users/1/gil_logging

// O
/users/1/gil-logging
```

### 대문자 대신 소문자를 사용한다.

```python
// X
/users/1/gilLogging

// O
/users/1/gil-logging
```


### Control Resoruce의 경우 동사형태를 허용한다.

**`Function`, `Control Resource`등을 나타내는 URL**은 **동작을 포함하는 동사 형태의 이름을 허용**한다.

즉, **HTTP Method(GET, POST, PUT, PATCH, DELETE)로 표현되는 행위**들인, **`조회`, `생성`, `전체 수정`, `단일 수정`, `삭제` 관련 행위 외에 다른 행위를 표현해야 하는 경우**,

**동사 형태를 허용**한다.


```python
//X
/gil-logging/1/duplicating

//O
/gil-logging/1/duplicate
```

### GET, POST, PUT, DELETE 4가지 기본 행위 Method는 기본적으로 제공한다.

 **HTTP Method(GET, POST, PUT, DELETE)로 표현되는 행위**들인, 
 
 **`조회`, `생성`, `수정`, `삭제` 관련 행위를 표현하는 Method들은 기본적으로 제공**한다.
_PUT(전체)의 경우 PATCH(단일)와 함께 제공하는 것이 좋다._

|Method|GET|POST|PUT|PATCH|DELETE|
|:--:|:--:|:--:|:--:|:--:|:--:|
|/users|사용자 전체 조회|사용자 생성|사용자 전체 수정|사용자 전체의 일부 속성 수정|사용자 전체 삭제|
|/users/gillog|사용자 `gillog` 상세 조회|405 ERROR|405 ERROR|사용자 `gillog` 수정|사용자 `gillog` 삭제|
_HTTP State Code 405 : 서버에서 허용되지 않는 방법_


### OPTIONS, HEAD, PATCH 3가지 추가 HTTP Method 제공은 완성도를 높여준다.

**`OPTIONS`는 현재 `End-Point`에서 제공 가능한 API의 Method를 응답**한다.
```python
OPTIONS /gil-logging/1

HTTP/1.1 200 OK
Allow: GET, PATCH, DELETE, OPTIONS, HEAD
```

**`HEAD`는 요청에 대한 Header 정보를 응답**한다.
_Body가 없다._
```python
HEAD /gil-logging

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
```


### PATCH와 PUT을 올바르게 사용한다.

**`PUT` 요청 시 요청을 일부분만 보낸 경우 나머지는 default 값으로 수정되는 게 원칙**이다. 

**`PUT`은 다음과 같이 바뀌지 않는 속성도 요청에 보내야 한다.**

```python
// X
PUT /users/1
{
    "level": 11
}
HTTP/1.1 200 OK
{
    "name": null,
    "level": 11
}
```

```python
// O
PUT /users/1
{
    "name": "gillog"
    "level": 11
}
HTTP/1.1 200 OK
{
    "name": "gillog",
    "level": 11
}
```

`PATCH`를 이용하여 원래의 목적대로 ‘level’만 변경하는 요청을 보낸다.

```python
PATCH /users/1
{
    "level": 11
}
HTTP/1.1 200 OK
{
    "name": "gillog",
    "level": 11
}
```

## HTTP Status

### 의미에 맞는 HTTP Status를 응답한다.


```python
//X
HTTP/1.1 200 OK
{
    "result": false
    "status": 400
}

//O
HTTP/1.1 400 Bad Reqeust
{
    "result": "Check Your Parameter :("
}
```

### HTTP Status 만으로 상태 에러를 나타낸다.

**`HTTP Status`를 응답 Body 안에 중복으로 표시할 필요 없다.**


```python
//X
HTTP/1.1 404 Not Found
{
    "code" : 404,
    "error_code": -765
}

//O
HTTP/1.1 404 Not Found
{
    "code" : -765,
    "more_info" : "https://gillog.com/errors/-765"
}
```

### 성공 응답은 200번대 Status Code를 사용한다.

**`200`은 성공**, **`201`은 요청에 성공 후 새로운 Resource를 생성**한 경우(`POST`, `PUT`, `PATCH`),

**`202`는 비동기 작업**과 같이 **Server에 요청은 유효**하지만 **아직 처리하지 않은 경우 응답으로 사용**한다.

```python
HTTP/1.1 202 Accepted
{
    "links": [
        {
            "rel": "self"
            , "method": "GET"
            , "href": "https://gillog.com/users/3"
        }
    ]
}

```

**`204`는 응답 Body가 필요 없는 Resource 삭제 요청(`DELETE`)에 응답으로 사용**한다.
_**`200` 응답 Body에 `null`, `{}`, `[]`, `false`로 응답을 주는것과 다르게**,
아예 **응답 Body가 없다.**_

### 실패 응답은 400번대 Status Code를 사용한다.

**`400`은 요청에 미리 정의된 Parameter 요구 사항을 위반할 때 사용**하고, 
**위반된 값이나 이유를 Body를 통해 알려준다.**

```python
HTTP/1.1 400 Bad Request
{
    "message": "`gil-logging` must be required, but no parameter requested"
}

HTTP/1.1 400 Bad Request
{
    "message": [
        {
            "error": "no parameter"
            , "location": "body"
            , "param": "gil-logging"
            , "value": null
        }
    ]
}
```

아래 표를 통해 **`400`번대 Error Status Code를 활용**한다.

|Status|설명|활용|
|:--:|:--:|:--:|
|401|Unauthorized|인증되지 않은 요청일 경우|
|403|Forhidden|요청은 유효, 접근 불가 Resource 요청의 경우.<br>해당 접근 권한이 일부 Resource 접근은 가능하지만, 접근 불가 전체 Resource등을 요청한 경우.|
|404|Not Found|존재하지 않는 Resource를 요청하는 경우 등|
|405|Method Not Allowed|`POST /users/:id` 등 해당 Method가 제공하지 않는 요청을 보낸 경우<br>`Allow: GET, PATCH, DELETE` 등 `HTTP Header`에 사용 가능한 Method를 함께 제공|
|409|Conflict|해당 요청 처리가 로직상 수행 불가능하거나 모순이 발생한 경우<br>[EX] `DELETE /users/gillog`를 로직 상 모든 사용자 정보를 먼저 제거 후<br> user를 삭제하는 로직인 경우 409 응답|
|429|Too Many Requests|Dos, Brute-Force-Attck등 비정상적인 많은 요청의 경우<br>429 응답 후 해당 IP의 요청 수를 제한.|

### 500번대 에러는 반드시 Handling 한다.

**API Server에서는 더더욱 `500`번대 Error를 노출해서는 안된다.**

반드시 **모든 발생 가능한 에러를 Handling** 한다.

## Auth, Security


### Dos, Brute-force 등 비정상적인 요청은 429 오류 응답을 사용한다.

**`Dos`, `Brute-Force-Attack` 등 비정상적인 많은 요청**이나,
**`/posts`에 특정 사용자가 의도적으로 서버 과부하를 생성**하는등 **API Server에 요청을 보낼 경우**,

**`429 Too Many Requests` 오류 응답과 함께 일정 시간 뒤 요청 가능 처리**를 한다.

```python
HTTP/1.1 429 Too Many Requests
Retry-After: 3600
```


### 인증관련 Resource를 요청 하는 작업은 429, 401 오류 응답을 사용한다.

**`/auth`와 같은 `OAuth`, `JWT` 인증 Resource 관련 API**나,

**`/login`과 같은 `ID`, `PassWord`를 이용한 로그인 관련 API**에서는,

**비정상적 요청**일 경우 **n시간 동안 n회 요청 가능하게 `429` 후 `Retry-After : n` 처리**를 하거나,

**n회만 요청 가능하도록 `401` 응답**으로 **해당 IP에서 더 이상 인증 관련 API 사용 불가 처리**와 함께 **특수 절차 로직을 추가하는 응답을 사용**한다.
_`Retry-After`과 다름_


## HATEOAS

### 응답 후 사용자 Resoruce 상태 전이를 위한 link를 함께 제공한다.

**`REST API`의 경우 `Request - Response` 구조**로 이루어져 있고,

**Response의 내용도 단순**하여 이것 만으로는 **사용자 `Resource` 상태 전이에는 부족**할 수 있다.

**`REST API`가 아닌 Web HTML 화면 환경**에서는 이를 위하여 **사용자 상태가 전이될 수 있는 `link`를 함께 제공**할 수 있다.


**`rel`을 통해 변경될 `Resource`의 상태 관계**를 나타낸다.
_**`self`는 현재 URL 스스로를 지칭하는 예약어**처럼 사용된다._

**`href`를 통해 요청 URL link를 제공**하고,

**`method`를 통해 요청시 Method 정보를 제공**한다.

```python
POST /users {"id": "gillog"}

HTTP/1.1 201 Created
{
    "seq": 111
    , "id": "gillog"
    , "created": "2021-06-22 08:50"
    , "links": [
        {
            "rel": "self",
            "href": "http://gillog.com/users/111",
            "method": "GET"
        },
        {
            "rel": "delete",
            "href": "http://gillog.com/users/111",
            "method": "DELETE"
        },
        {
            "rel": "update",
            "href": "http://gillog.com/users/111",
            "method": "PATCH",
            "more_info": "http://gillog.com/docs/user-update"
            "body": {
                "name": "{The value to be modified}"
            }
        },
        {
            "rel": "user.gillog",
            "href": "http://gillog.com/users/1/gil-log",
            "method": "GET"
        }
    ]
}

```

## Parameter

### Parameter를 활용하여 Paging 처리를 한다.

**`page`, `per_page`, `limit`, `offset` 등 parameter 를 선언**하고,

**해당 Parameter를 활용해 Paging 처리**를 한다.
_**필수 여부에 따라 Resource 과부하를 조절**할 수 있다._

|Parameter|설명|
|:--:|:--:|
|page|요청 페이지|
|per_page<br>limit<br>offset|페이지 당 응답 Resource 수|


```python
GET /users

HTTP/1.1 200 OK
Link: 
 <https://gillog.com/users?offset=10&limit=10>; rel="next",
 <https://gillog.com/users?offset=50&limit=10>; rel="last",
 <https://gillog.com/users?offset=0&limit=10>; rel="first",
 <https://gillog.com/users?offset=0&limit=0>; rel="prev",
[
    {index: 1
    , ...},
    {index: 2
    , ...},
    ...
    {index: 10
    ,...},
]
```

### 검색조건의 parameter의 양이 길어지면 가독성이 떨어진다.

만일 아래와 같은 REST API가 있다고 가정하자.

```python
GET /users?name=gil&id=gillog&fields=name,region,tel&page=1&limit=10
```
`users` Collection에서 name이 gil, id가 gillog인,
사용자의 정보 중 field 값 중 name, info, tel만 가져오고,
포함된 정보가 다량인 경우라 pagination이 필요하다고 하면 위 처럼
URL이 길어진다.

만약 **검색이 필요한 경우 하나의 `Query String`으로 검색 조건을 별도로 정의**하여 사용하는 것으로 **검색 조건과, pagination 조건**을 구분지을 수 있다.

```python
GET /users?q=name=gil,id=gillog&fields=name,region,tel&page=1&limit=10
```

## Collection

### Collection은 복수형, Element는 단수형으로 URL에 표현한다.


**`Collection`의 경우 복수형을, `Element`의 경우 단수형을 통해 URL에 표현**한다.

```python
GET /licenses/sqlp/questions/11
```

위 예제 API는 **`licenses` Collection에 있는 `sqlp` Elements에 있는 `questions` Collection에서 11번 문제를 얻고 싶은 REST API**이다.

### Collection에 대한 요청의 경우 Parameter를 통해 정렬 기준을 사용한다.

**`GET /users`와 같이 `Collection`에 대한 정보를 요청**할 경우,

**Parameter에 `order` 등의 Parameter를 활용**하여 **정렬 기준을 통해 정렬한 결과를 응답**한다.

```python
GET /users?order=created

HTTP/1.1 200 Ok
Link: 
 <https://gillog.com/users?offset=10&limit=10&order=created>; rel="next",
 <https://gillog.com/users?offset=50&limit=10&order=created>; rel="last",
 <https://gillog.com/users?offset=0&limit=10&order=created>; rel="first",
 <https://gillog.com/users?offset=0&limit=0&order=created>; rel="prev",
[
    {index: 1
    , ...},
    {index: 2
    , ...},
    ...
    {index: 10
    ,...},
]
```

### Collection 요청의 경우 특정 Field를 선택해 응답 받을 수 있다.

**Paremter에 Collection에서 원하는 Resource Filed를 요청**에 함께 보낼 경우,

**해당 Resource에 대한 값만 응답으로 제공**한다.

```python
GET /users?fields=name/111
HTTP/1.1 200
{
    "name": "gillog"
}

GET /users?fields=id,name/111
HTTP/1.1 200
{
    "id": "gillog"
    , "name": "gillog"
}

GET /users?fields=name, created/111
HTTP/1.1 200
{
    "name": "gillog"
    , "created": "2021-06-22 08:50"
}
```