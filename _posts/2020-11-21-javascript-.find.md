---
title: "[javascript] .find()"
last_modified_at: 2020-11-21T13:03
categories: 
  - javascript
tags: 
  - '.find()' 
  - 'JavaScript'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# .find()

ES5에서는 주로 `Array.prototype.foreach()`, `Array.prototype.map()`, `Array.prototype.filter()` 를 사용하여 특정 또는 전체 값에 대한 처리를 했다.

하지만 **이 method들의 단점은 처리를 하는 도중 이 모든 함수는 한 배열 안에 있는 모든 데이터를 확인 한 후 변경 및 삭제**를 한다.

**즉 어떤 하나의 특정 값만 바꾸려고 하는데 모든 데이터를 확인 한다는 점**이다. 


**ES6에서 이러한 단점을 보완하고자 `Array.prototype.find()` method가 나왔다. **

<br>

**`.find()` method는 주어진 판별 함수를 만족하는 첫 번째 요소의 값을 반환**한다.

**그런 요소가 없다면 undefined를 반환**한다.

``` javascript
const array1 = [5, 12, 8, 130, 44];

const found = array1.find(element => element > 10);

//output: 12
console.log(found);
```

**이전 method들과의 차이점은 속도는 물론 반환 데이터 타입이 다르다.**

**`Array.prototype.filter()`는 반환 값이 Array**이고, **`Array.prototype.find()`는 반환 값이 해당하는 값 (Number)으로 반환**한다.

```javascript
const arrayData = [1, 2, 3, 4, 5];

// filter 사용
// console.log() => [3] 
arrayData.filter(dataNum => dataNum === 3); 

// find 사용
// console.log() => 3
arrayData.find(dataNum => dataNum === 3); 

// find를 사용하는데 찾는 값이 없을 때
// console.log() => undefined
arrayData.find(dataNum => dataNum === 9); 
```

또한 **`.find()`는 `.filter()`와 다르게 찾고 싶은 요소를 검색한 후에는 즉시 검색을 종료**한다.

```javascript
const arrayData = [1, 2, 3, 4, 5];
let count = 0;

// filter 사용
arrayData.filter(dataNum => {
  count++;
  return dataNum === 3;
});

// count 값 : 5


const arrayData = [1, 2, 3, 4, 5];
let count = 0;

// find 사용
arrayData.find(dataNum => {
  count++;
  console.log(count);
  return dataNum === 3;
});

// count 값 : 3
```

**`.find()`의 장점은 큰 데이터를 처리할 때 검색 속도가 빠르다는 것이고, 단점은 여러개의 데이터를 검색할 때는 활용하기가 까다롭다.**

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[Array.prototype.find()[MDN web docs]](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

[JavaScript 배열 안에 특정한 값을 찾을 때 사용하기 좋은 find 메소드 (ES6)[Happy Coding]](https://happycording.tistory.com/entry/JavaScript-%ED%8A%B9%EC%A0%95-%EA%B0%92-%EC%B0%BE%EC%9D%84-%EB%95%8C-%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9D%B8-%EB%B0%A9%EB%B2%95)

[]()

[]()

[]()

[]()