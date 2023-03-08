---
title: "[javascript] var, let, const"
last_modified_at: 2020-11-15T12:03
categories: 
  - javascript
tags: 
  - 'JavaScript' 
  - 'const' 
  - 'let' 
  - 'var'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

**es5까지 Javascript에서는 변수 선언할때 `var`를 사용**하였다.

**`var`를 선언하여 변수를 선언할때 잘못된 사용으로 인해 문제가 발생하는 경우를 보완하기위해 Javascript es6로 올라가게 되면서 `let`과 `const`가 추가**되었다.

# var(function-scoped)

**`var`는 function-scoped**이다.


```javascript
// var는 function-scope이기 때문에 for문이 끝난다음에 i를 호출하면 값이 출력이 잘 된다.
// 이건 var가 hoisting이 되었기 때문이다.
for(var j=0; j<10; j++) {
  console.log('j', j)
}
console.log('after loop j is ', j) // after loop j is 10


// 아래의 경우에는 에러가 발생한다.
function counter () {
  for(var i=0; i<10; i++) {
    console.log('i', i)
  }
}
counter()
console.log('after loop i is', i) // ReferenceError: i is not defined
```

위와같이 **function을 만들어서 호출하기 싫다면 `IIFE`로 function-scope인거 처럼 만들 수도 있다.**
_javascript에서는 `immediately-invoked function expression` (or `IIFE`, pronounced "iffy")라는것이 있다._

```javascript
// IIFE를 사용하면
// i is not defined가 뜬다.
(function() {
  // var 변수는 여기까지 hoisting이 된다.
  for(var i=0; i<10; i++) {
    console.log('i', i)
  }
})()
//여기까지가 IIFE
console.log('after loop i is', i) // ReferenceError: i is not defined
```

**하지만 `IIFE`는 function-scope처럼 보이게 만들어주지만 결과가 같지는 않다.**

```javascript
(function() {
  for(i=0; i<10; i++) {
    console.log('i', i)
  }
})()
console.log('after loop i is', i) // after loop i is 10
```
**위 코드가 아무 에러 없이 실행되는 이유**는 **`i`가 hoisting이 되어서 global variable이 되었기 때문**에 아래와 같이 된 것이다.
_ `hoisting` : 함수 안에 있는 선언들을 모두 끌어올려서 해당 함수 유효 범위의 최상단에 선언하는 것_

```javascript
var i
(function() {
  for(i=0; i<10; i++) {
    console.log('i', i)
  }
})()
console.log('after loop i is', i) // after loop i is 10
```
**`IIFE`를 사용하는데 이런 hoisting을 막기 위해 `use strict`를 사용**한다.

```javascript
// 아까랑 다르게 실행하면 i is not defined라는 에러가 발생한다.
(function() {
  'use strict'
  for(i=0; i<10; i++) {
    console.log('i', i)
  }
})()
console.log('after loop i is', i) // ReferenceError: i is not defined
```



# let, const(block-scoped)

`let`, `const`는 es6부터 추가되어 아래와 같은 `var`에 존재했던 문제들을 보완해준다.

```javascript
// 이미 만들어진 변수이름으로 재선언했는데 아무런 문제가 발생하지 않는다.
var a = 'test'
var a = 'test2'

// hoisting으로 인해 ReferenceError에러가 안난다.
c = 'test'
var c
```

**`let`과 `const`의 공통점**은 **`var`와 다르게 변수 재선언 불가능**이다.

**`let`과 `const`의 차이점**은 **변수 `immutable`여부**로, **`let`은 변수 재할당이 가능해 변수로써 사용** 가능하고, **`const`는 변수 재할당이 불가능하여 상수로써 사용**한다.


```javascript
// let
let a = 'test'
let a = 'test2' // Uncaught SyntaxError: Identifier 'a' has already been declared
a = 'test3'     // 가능

// const
const b = 'test'
const b = 'test2' // Uncaught SyntaxError: Identifier 'a' has already been declared
b = 'test3'    // Uncaught TypeError:Assignment to constant variable.
```


**`let`, `const`도 hoisting이 발생**하는데,**`var`가 function-scoped로 hoisting**이 되었다면, 
**`let`, `const`는 block-scoped단위로 hoisting**이 일어난다.

스코프에 진입할 때 **변수가 만들어지고 TDZ(Temporal Dead Zone)가 생성**되지만, **코드 실행이 변수가 실제 있는 위치에 도달할 때까지 액세스할 수 없는 것**이다.

**`let`, `const` 변수가 선언된 시점에서 제어흐름은 TDZ를 떠난 상태가 되며, 변수를 사용할 수 있게 된다.**


```javascript
c = 'test' // ReferenceError: c is not defined
let c	  //ReferenceError가 발생한 이유는 tdz(temporal dead zone)때문
```
**`let`은 값을 할당하기전에 변수가 선언 되어있어야 하는데 그렇지 않기 때문에 에러**가 발생하였다.

**`const`는 좀 더 엄격하게 변수 선언과 동시에 값이 할당되어야 한다.**

```javascript
// let은 선언하고 나중에 값을 할당이 가능하다.
let dd
dd = 'test'

// const 선언과 동시에 값을 할당 해야한다.
const aa // Missing initializer in const declaration
```


# var vs let, const 차이점 정리

**`var`의 경우 두 번이상 선언이 가능**하지만 
**`let`, `const`의 경우 두번이상 선언하게 될 경우 오류**가 발생한다.

<br>

**`var`의 경우 전역 변수를 선언할 경우에 global object의 properties로** 들어가게 된다.

**선언된 global object를 사용하기 위해서 this나 window 키워드를 통해 사용**한다.

**하지만 `let`이나 `const`의 경우에는 global object의 properties를 생성할 수 없다.**

<br>

**`let`과 `const`는 재선언이 안되는 특징**으로 **`let`은 값 재할당이 가능하여 변수로 사용**하고,
**`const`는 값 재할당이 불가능해 상수를 선언할 때 사용**한다.
_array, object, function등을 const로 많이 선언_


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[var와 let 그리고 const 차이점과 사용법 소개[wedul]](https://wedul.site/143)

[var, let, const 차이점은?[LeoHeo]](https://gist.github.com/LeoHeo/7c2a2a6dbcf80becaaa1e61e90091e5d)

[let과 const는 호이스팅 될까?[yocee57]](https://medium.com/korbit-engineering/let%EA%B3%BC-const%EB%8A%94-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-%EB%90%A0%EA%B9%8C-72fcf2fac365)

[]()

[]()

[]()