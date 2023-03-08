---
title: "[JS] Promise"
last_modified_at: 2021-10-26T03:20
categories: 
  - javascript
tags: 
  - 'JavaScript' 
  - 'promise'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[Promise [MDN Web Docs]](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

---

# Promise

**`Promise` 객체**는 **비동기 작업이 처리 된 후 미래의 성공, 실패, 그 결과 값**을 나타낸다.



**`Promise`는 생성될 때 확실하지 않은 미래의 결과 값을 위한 대리자**로,

**비동기 연산이 종료된 이후**에 **결과 값**이나, **실패 이유를 처리**하기 위한 **handler들을 연결 할 수(Chaining) 있도록** 한다.

<br>


**코드로 쉽게 알아보자**면,

**아래와 같은 `.then`, `.catch`, `.finally` 와 같은 Chaining된 handler들**이,

**axios와 같은 비동기 연산을 처리할때 자주 사용**된다.

```javascript
axios.get('/gillog', {
    params: {
      ID: 1
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });  
```

이는 **다음과 같은 비동기 요청이 실행 된 이후에 `axios.get('/gillog', {params: {ID:1}});`**,


**정상적으로 실행이 완료되면 `.then`**을 통해,

**비정상적으로 실행이 되어 에러가 발생할 경우 `.catch`를 통해 처리하는 형태**이다

<br>

**`Promise`를 사용**함으로 써 **비동기 메소드를 마치 동기 메소드 처럼 값을 반환하며 사용할 수 있게** 해준다.


다만 **최종 결과를 반환하지는 않고**, **`Promise`를 반환**하여 **미래 특정 시점(정상, 에러, ..)에 결과를 반환**한다.


---

## Promise 상태

**`Promise`는 아래 세 가지 중 하나의 상태**를 가진다.

- `대기(Pending)` : 이행하거나 거부되지 않은 초기 상태

- `이행(Fulfilled)` : 연산이 성공적으로 완료

- `거부(Rejected)` : 연산이 실패


**`대기(Pending)` 중인 `Promise`**는 **값과 함께 `이행(Fulfilled)` 될 수도**, 

**오류로 인해 `거부(Rejected)`될 수도** 있다.

<br>

**`이행(Fulfilled)`, `거부(Rejected)` 될때 `Promise`에 연결한 처리기**는 **`.then()` method에 의해 대기열에** 오른다.

**이미 `이행(Fulfilled)`했거나, `거부(Rejected)`된 `Promise`에 연결한 처리기도 호출**하므로,

**비동기 연산과 처리기 연결 사이에 경합 조건은 없다.**

<br>


![](https://images.velog.io/images/gillog/post/53fbee80-a788-4d88-ad62-d51099cf682e/image.png)


**`Promise.prototype.then()`** 과 **`Promise.prototype.catch()`**의 **반환 값**은 **별도의 다른 `Promise`**로

**서로 Chaining하여 사용할 수 있다.**

---

## Promise Method

### Promise.all(iterable)

**`iterable` 내 모든 `Promise`가 이행한 뒤 이행**하며, 

**특정 `Promise`가 거부하는 즉시 해당 `Promise`를 반환**한다.

<br>

**반환된 `Promise`가 이행하는 경우**, **`iterable` 내 `Promise`가 결정한 값을 모은 배열이 이행 값**이 된다.

**반환된 `Promise`가 거부하는 경우**, **`iterable`내 거부한 `Promise`의 이유를 모은 배열이 이행 값**이 된다.


**`Promise.all`은 결국 여러 `Promise`의 결과를 모을 때 사용**한다.


### Promise.race(iterable)

**`iterable` 내의 특정 `Promise`가 이행, 거부 하는 즉시 스스로 이행** 하거나,

**거부하는 `Promise`를 반환**한다.


**이행 값이나 거부 이유**는 **반환 `Promise`의 값이나 이유를 그대로 사용**한다.


### Promise.reject()

**해당 이유로 거부하는 `Promise` 객체를 반환**한다.

### Promise.resolve()

**해당 값으로 이행하는 `Promise` 객체를 반환**한다.

**값이 `.then` 가능한(`.then` method가 있는) 경우**,

**반환된 `Promise`**는 **`.then` method를 따라가고 마지막 상태**를 띈다.

**`.then`이 불가능한 경우**,  **반환된 `Promise`는 주어진 값으로 이행**한다.

<br>

**만약 어떤  값이 `Promise`인지 아닌지 모를 경우**에,

**`Promise.resolve(value)` 실행 후 반환 값을 `Promise`로 처리할 수 있다.**


---

# Promise 예제

## Basic

**`Promise`를 사용하는 script를 코드와 함께 살펴보면 아래**와 같다.


```javascript
let firstPromise = new Promise((resolve, reject) => {
  setTimeout(function(){
    resolve("log!");
  }, 250);
});

firstPromise.then((successMessage) => {
  console.log("Gil! " + successMessage);
});

// after 2.5초
// Gil! log!
```

**위 `firstPromise`**는 **`Promise` 생성자로 생성된 `Promise` 객체**로,

**이행되는 결과**는 **2.5초 이후에 `log!` 라는 값을 반환하며 이행**한다.

**그 후 해당 `Promise`객체**인 **`firstPromise.then` chaining**을 통해,

**해당 비동기 연산(` resolve("log!");`)**를 **이행한 후**에 **console.log()로 "Gil! " 와 함께**,

**비동기 연산 결과를 로그 찍는 간단한 `Promise` 실행 예제**이다.


## NextLevel


```javascript
var promiseCount = 0;

function testPromise() {
    var thisPromiseCount = ++promiseCount;
  
    // [1]
    console.log("동기 연산 실행 시작!");

    // 새 프로미스 생성 - 프로미스의 생성 순서를 전달하겠다는 약속을 함 (3초 기다린 후)
    var p1 = new Promise(
        function(resolve, reject) {
            // [2]
            console.log("비동기 연산 시작!");
            window.setTimeout(
                function() {
                  
                    // [4]
                    // 프로미스 이행(resolve) !
                    // 해당 프로미스 카운트 반환
                    resolve(thisPromiseCount);
                }, Math.random() * 2000 + 1000);
        }
    );

    // 프로미스를 이행했을 때 할 일은 then() 호출로 정의하고,
    // 거부됐을 때 할 일은 catch() 호출로 정의
    p1.then(
        function(val) {
            // [5]
            console.log("비동기 연산 이행 이후 then 실행 ! 결과 = " + val);
        })
    .catch(
        function(reason) {
            console.log('거부된 프로미스(' + reason + ')를 처리.');
        });
  
    // [3]
    console.log("동기 연산 실행 종료!");
}
```
위 **`testPromise()` method**는 **`Promise` 객체를 포함하는 method**로

**해당 `testPromise()` 실행** 시 **`Promise` 비동기 연산과 동기 연산의 동작 과정을 나타내주는 method** 이다.

<br>

### 동작 과정

먼저 **전역으로 선언한 `promiseCount`**를 **`++promiseCount` 하여 `thisPromiseCount`에 저장**하고,

**`console.log("동기 연산 실행 시작!");`를 통해 동기 연산 과정에 실행**을 알린다.

**그 후 `p1`이라는 `Promise` 객체를 생성**하는데,

**해당 객체**는 **`console.log("비동기 연산 시작!");`를 통해 `Promise` 객체 안에서 비동기 연산이 실행되는 시점**을 알려주고,

**`Math.random() * 2000 + 1000`인 랜덤한 시간 이후**에 **해당 `Promise`를 resolve()를 통해 `이행(resolve)`** 한다.

**이행 결과 값으로 `thisPromiseCount` 값을 반환**하고,

**해당 `Promise` 객체 `p1`**에 **`.then()`(이행 후 실행 될 chaining 연산)**으로는,

**이행 결과 값과 함께 `console.log("비동기 연산 이행 이후 then 실행 ! 결과 = " + val);`를 log 찍도록 선언**되어 있다.

그리고 **`console.log("동기 연산 실행 종료!");` 와 함께 `testPromise()`의 실행은 종료**된다.



**실제 실행 결과는 아래**와 같다.

![](https://images.velog.io/images/gillog/post/2ea7ec30-39c9-49c4-868e-1f2b4af23f66/image.png)

_**`[1]` > `[2]` > `[3]` > `[4]` > `[5]` 순**_


이를 통해 **`Promise`를 사용할 때 동기, 비동기 연산의 동작 과정**을 알 수 있다.

