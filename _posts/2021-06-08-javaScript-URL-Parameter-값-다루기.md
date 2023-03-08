---
title: "[javaScript] URL Parameter 값 다루기"
last_modified_at: 2021-06-08T07:35
categories: 
  - javascript
tags: 
  - 'JavaScript' 
  - 'parameter' 
  - 'url'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[URLSearchParams[MDN Web Docs]](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams#browser_compatibility)

<br>

---


**순수 `javaScript`에서 URL과 Parameter를 다루어야 할 때**가 종종 있다.

아래를 살펴보자.

---

# URL 주소 얻기

기본 객체 `window`에 **`window.location.href`를 이용하면 현재 페이지의 URL**을 얻을 수 있다.

```javascript

const url = window.location.href;

console.log(url);
// https://velog.io/@gillog/javaScript-URL-Parameter-%EA%B0%92-%EB%8B%A4%EB%A3%A8%EA%B8%B0
```
---


# URL Parameter(QueryString) 다루기

기본 객체 `window`에 **`window.location.search`를 이용하면 현재페이지 URL의 Parameter**를 얻을 수 있다.

```javascript
// url = https://gillog/post/1?tag=javascript&like=backend
const urlParameter = window.location.search;

// ?tag=javascript&like=backend
console.log(urlParameter);
```

## 특정 URL Parameter 얻기

**`new URL("URL 문자열")`을 이용**하면, 손쉽게 **URL의 특정 Parameter를 얻을 수 있다.**


```javascript
const urlStr = 'https://gillog/post/1?tag=javascript&like=backend';
const url = new URL(urlStr);
```

### url.searchParams

**URL 객체의 `.searchParams` 속성은 `URLSearchParams` 객체를 return**한다.


### urlSearchParams.get("parameterName");


`urlSearchParams`에 **`.get('parameterName')`은 해당 `parameterName`으로 조회되는 첫번째 값을 return**한다.

### urlSearchParams.getAll("parameterName")

`urlSearchParams`에 **`.getAll("parameterName")`은 `parameterName`으로 조회되는 모든 값을 배열로 return**한다.

```javascript
const urlStr = 'https://gillog/post/1?tag=javascript&like=backend';
const url = new URL(urlStr);

const urlParams = url.searchParams;

const tag = urlParams.get('tag');

// javascript
console.log(tag);

const like = urlParams.get('like');

// backend
console.log(like)
```

---

## 전체 URL parameter 얻기


### urlSearchParams.keys()

**`urlSearchParams`에 `.keys()`는 parameter에 해당하는 `key`들을 순회할 수 있는 `iterator`를 return**한다.

```javascript
const urlParams = new URLSearchParams("?gil=test&log=what&gillog=holy");

const keys = urlParams.keys();


// gil
// log
// gillog
for(const key of keys) {
  console.log(key);
}
```

### urlSearchParams.values()

**`urlSearchParams`에 `.values()`는 parameter에 해당하는 `value`들을 순회할 수 있는 `iterator`를 return**한다.


```javascript
const urlParams = new URLSearchParams("?gil=test&log=what&gillog=holy");

const values = urlParams.values();

// test
// what
// holy
for(const value of values) {
  console.log(value);
}
```

### urlSearchParams.entries()

**`urlSearchParams`에 `.entries()`는 parameter에 해당하는 `key`와 `value`들을 순회할 수 있는 `iterator`를 return**한다.


```javascript
const urlParams = new URLSearchParams("?gil=test&log=what&gillog=holy");

const entries = urlParams.entries();

// gil ! test
// log ! what
// gillog ! holy
for(const entry of entries) {
  console.log(entry[0] + ' ! ' + entry[1]);
}
```


---

## URL Parameter 존재 여부

`urlSearchParams`에 **`.has("parameterName")`를 사용하면, `parameterName`에 해당하는 Parameter가 있는지 확인**할 수 있다.


```javascript
const urlParams = new URLSearchParams("?gil=123&log=456&gillog=777");

// true
console.log(urlParams.has("gillog"));

// false
console.log(urlParams.has("loggil"));
```

---

## URL Parameter 추가

**`urlSearchParams`에 `.append("parameterName", "value")`을 사용하면 parameter를 추가**할 수 있다.

이때 **`parameterName`이 중복된다면 parameter 값이 계속 추가** 된다.

```javascript
let urlParams = new URLSearchParams("?gil=test");

urlParams.append("log", "yes");
urlParams.append("log", "no");

// ?gil=test&log=yes&log=no
console.log(urlParams);
```

---

## URL Parameter 변경
**`urlSearchParams`에 `.set("parameterName", "value")`를 사용하면 ,
`parameterName`의 value를 변경**할 수 있다.

만약 **존재하지 않는 `parameterName`이라면 append** 된다.

만약 **`parameterName`으로 중복되는 값이 있을 경우 1개만 변경된 값으로 남기고 모두 제거**된다.

```javascript
let urlParams = new URLSearchParams("?gil=test&log=yes&log=no");

urlParams.set("gil", "yes");
urlParams.set("log", "wow");
urlParams.set("gillog", "good");

// ?gil=yes&log=wow&gillog=good
console.log(urlParams);
```

---

## URL Parameter 삭제

`urlSearchParams`에 **`.delete("parameterName")`를 이용하면 `parameterName`을 제거**할 수있다.

**`parameterName`으로 중복된 값이 존재해도 모두 제거**한다.


---
