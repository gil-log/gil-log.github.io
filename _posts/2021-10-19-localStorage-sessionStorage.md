---
title: "localStorage, sessionStorage"
last_modified_at: 2021-10-19T08:59
categories:
  - concept
tags: 
  - 'localstorage' 
  - 'sessionStorage'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import

[localStorage와 sessionStorage[JAVASCRIPT.INFO]](https://ko.javascript.info/localstorage)

[Window.sessionStorage[MDN Web Docs]](https://developer.mozilla.org/ko/docs/Web/API/Window/sessionStorage)


[Local Storage vs Session Storage vs Cookies
[코딩하는 폴제트]](https://zakelstorm.tistory.com/5)

[로컬스토리지, 세션스토리지[ZeroCho TV]](https://www.zerocho.com/category/HTML&DOM/post/5918515b1ed39f00182d3048)

---

# localStorage

**`localStorage`**는 **`sessionStorage`와 함께 `HTML5`에 추가된 저장소**이다.

**간단한 데이터들을 `Key`, `Value` 형태로 저장**할 수 있다.


**`localStorage`와 `sessionStorage`의 차이는 데이터의 영구성**이다.

**`localStorage`의 데이터**는 **사용자가 브라우저에서 지우지 않는 이상 계속해서 남아있다.**


**저장된 데이터는 브라우저 세션간 공유** 된다.

<br>

그래서 보통 **지속적으로 필요한 데이터(자동 로그인 여부 등)는 `localStorage`에 저장**하고,

**일시적으로 필요한 정보(일회성 정보 등)**은 **`sessionStorage`에 저장**한다.



<br>


**`Document` 출처의 `Storage` 객체에 존재**한다.
_**`window` 객체에 포함**_

**`localStorage`에 저장되는 값**은 **문자열, Boolean, Number, null, undefined 등을 저장할 수 있지만**,

모두 **문자열로 변환되어 저장**된다.
_**Key도 문자열로 변환**_

<br>





# sessionStorage


**`localStorage`는 `sessionStorage`와 비슷**하지만,

**`localStorage`의 데이터는 만료되지 않고**,

**`sessionStorage`의 데이터는 페이지 세션이 끝나는 시점(페이지를 종료할 때) 데이터가 삭제**된다.


## vs Cookie

**`localStorage`와 `sessionStorage`가 나오기 전 부터 `Cookie`라는 저장소가 존재**했다.

**`Cookie`는 만료 기한이 있는 `key-value` 저장소**이다.

```javascript
// _ga=GAX.X.XXXXXXXXXXX; _gid=GAX.X.XXXXXXXXX; _gat=1
document.cookie;
```
**`Cookie`의 경우 `httponly` Flag를 사용**할 경우 **`javascript` 단에서 쓸 수 없게 제한**할 수 있다.


![](https://images.velog.io/images/gillog/post/aaf3d462-9b67-487e-9c06-d43fa6930bc0/image.png)

<br>

**`Cookie`는 Server와 Client의 지속적인 데이터 교환을 위해 만들어 졌기 때문**에,

**서버로 Request 마다 계속 전송**된다.

**`Cookie`의 용량 제한은 `4kb`로 불필요한 정보가 `Cookie`로 저장될 경우**,

**매 Request마다 `4kb` 용량 안에서 지속적으로 요청**되게 된다.

**이를 해결**하기 위해 **지속적으로 포함되지 않아도 되는 데이터들**을

**`localStorage`, `sessionStorage`에 저장하기 위해 `Storage`가 `HTML5`에 추가**되었다.


<br>

**`Cookie`와 또 다른 점**은 **서버가 HTTP Header를 통해 스토리지 객체를 조작할 수 없다**.
_**Storage 객체는 모두 javascript에 의해서 수행**_

<br>

**Storage 객체**는 **`Domain`, `Protocol`, `Port`로 정의되는 `Origin`에 묶여**있다. 

**`Protocol`과 `sub domain`이 다르면 데이터에 접근할 수 없다.**



## methods

**`localStorage`와 , `sessionStorage`는 `setItem()`, `getItem()`, `removeItem()`, `clear()` 네 가지 method를 사용**할 수 있다.

**둘의 차이**는 **`localStorage`는 영구적으로 보관된다는 점**이다.
_**사용자가 지우지 않는 이상 브라우저에 계속 저장**_
_**sessionStorage**는 **Window나 브라우저 탭을 닫을 경우 제거**_

```javascript
// Storage에 저장
window.localStorage.setItem("key-gillog", "value1");

// Storage에서 조회
// value1
window.localStorage.getItem("key-gillog");

// Storage에서 삭제
window.localStorage.removeItem("key-gillog");

// null(removed)
window.localStorage.getItem("key-gillog");

// 전체 삭제
window.localStorage.clear();
```


## Object 저장

**`Storage`에 Value**는 **`toString()` method가 호출된 형태로 저장**된다.

**`Object` 형태로 저장하기 위해서**는 **`Key-Value` 형식을 풀어서 여러 번 나누어 저장** 하거나,

**`Object` 형식으로 한번에 저장하기 위해서**는 **`JSON.stringify`를 사용**해 **`Object` 형식 그대로를 문자열로 변환해 저장** 한 후,

**출력할 때 `JSON.parse`를 사용하는 방법**이 있다.


```javascript
const obj = { gil: 'log' };

window.localStorage.setItem("object-key-gillog", JSON.stringify(obj));

// { gil: 'log' }
JSON.parse(window.localStorage.getItem("object-key-gillog");
```

