---
title: "[jQuery] Selector(선택자)"
last_modified_at: 2020-11-22T22:28
categories: 
  - javascript
tags:
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Selector

javascript Library중 **jQuery를 선택하는 가장 큰 이유가 `jQuery Selector(선택자)` 때문**이다.

**`jQuery Selector(선택자)`는 css Selector를 그대로 사용할 수 있어 코드 선택이 매우 간단**하다.


ES6에서 부터 `document.querySelector`와 `document.querySelectorAll`이 나와 jQuery Selector의 가치가 떨어졌지만 구형 브라우저에서는 아직도 유용하다.

**`jQuery Selector`는 $( ) 모양**인데, 이것의 의미는 **$라는 변수(함수)의 괄호 안에 인자를 제공**하는 것이다.

아래는 jQuery에서 사용 가능한 Selector들이다.

## #(id)

**`#`은 해당 id를 가진 태그를 선택**하는 선택자이다.

- 기존 js

```javascript
document.getElementById('gil');
```

- ES6

```javascript
document.querySelector('#gil')

```
- jQuery

```javascript
$('#gil');
```

## .(Class)

**`.`은 해당 Class를 선택**하는 선택자이다.
- 기존 js
```javascript
document.getElementsByClassName('gil');
```
- ES6
```javascript
document.querySelectorAll('.gil')
```
- jQuery
```javascript
$('.gil');
```

## Tag

**단순히 tag명(EX : p, a ..)을 입력하면 해당 태그들을 선택**할 수 있다.

- 기존 js
```javascript
document.getElementsByTagName('p');
```

- jQuery
```javascript
$('p');
```

## `[](속성)`

**`[]`는 tag 속성으로 선택**할 수 있는 선택자이다.

- 기존 js
```javascript
<!-- 없음 -->
```
- ES6
```javascript
document.querySelectorAll('[value]')
document.querySelectorAll('[value="gil"]')
```

- jQuery
```javascript
$('[value]'); // value라는 속성을 가진 태그
$('[value="gil"]'); // value속성이 gil인 태그
```

## >(자식 선택자)

**`>`는 해당 tag의 자식요소를 선택**하는 선택자이다.

- 기존 js
```javascript
Array.prototype.filter.call(document.getElementById('gil').children, function(item) {
  return item.tagName === 'P';
});
```
- ES6
```javascript
document.querySelectorAll('#zero > p');
```

- jQuery
```javascript
$('#zero > p'); // #zero 태그의 자식인 p 태그를 선택
```

## 후손 선택자

**후손이란 해당 tag 아래에 포함된 모든 tag**를 일컫는다. 
_**자식 tag는 해당 태그 바로 아래의 tag**만 해당_


- 기존 js
```javascript
document.getElementById('gil').getElementsByTagName('div');
```
- ES6
```javascript
document.querySelectorAll('#gil div');
```

- jQuery
```javascript
$('#gil div');
```

## +, ~ (형제자매 선택자)

**`+` 는 다음 하나의 형제 자매 tag를 선택**하고, **`~`는 형제 자매 tag 중 일치하는 것을 모두 선택**하는 선택자이다.

- 기존 js
```javascript
<!-- 없음 -->
```
- ES6
```javascript
document.querySelectorAll('#gil + div');
document.querySelectorAll('#gil ~ p');
```

- jQuery
```javascript
$('#gil + div'); // #zero 바로 다음 div를 선택합니다.
$('#gil ~ p'); // #zero의 형제자매 p를 모두 선택합니다.
```

## : (가짜 선택자)

`:`는 pseudo selector라고 불리는 선택자들이다.

`:`앞에 다른 선택자를 붙여서 조합할 수 있다. 

_EX :  ```#gil:parent ```_


- jQuery
```javascript
// 각각 모든 인풋, 버튼, 이미지, 체크박스, 라디오, 텍스트인풋 태그를 선택
$(':input'), $(':button'), $(':image'), $(':checkbox'), $(':radio'), $(':text'); 

// 각각 홀수 번째, 짝수 번째 태그, 주어진 순서보다 순서가 큰 태그, 순서가 작은 태그, 선택된 것들 중에 마지막 태그를 선택
$(':odd'), $(':even'), $(':gt(순서)'), $(':lt(순서)'), $(':last') 

// 각각 현재 포커스된 태그, 자식이 하나라도 있는 태그, 자식이 없는 태그, 비활성화된 태그, 활성화된 태그, 투명이 아닌, 숨겨진 태그를 선택
$(':focus'), $(':parent'), $(':empty');, $(':disabled'), $(':enabled'), $(':visible'), $(':hidden'); 

 // 각각 보여지는(visible), 체크된(체크박스), 선택된(select), 부모의 유일한 자식인 태그, 첫 번째 자식인 태그, 마지막 자식인 태그를 선택
$(':visible'), $(':checked'), $(':selected'), $(':only-child');, $(':first-child'), $(':last-child')


// 각각 순서 번째 자식 태그, 해당 타입 중 순서 번째 자식 태그, 순서 번째 태그를 찾는다. 
// eq는 0부터 시작하고, nth-child는 1부터 시작.
$(':nth-child(순서)'), $(':nth-of-type(순서)'), $(':eq(순서)'); 

// 각각 선택자를 자식으로 갖고 있는 태그나 해당 텍스트를 갖고 있는 태그를 선택.
$(':has(선택자)'), $(':contains(텍스트)');
```


## 조합

위 jQuery 선택자들은 조합하여 사용할 수 있다.

```javascript
// tag 이름이 p, id가 gil, checkbox인 tag 선택
$('p#gil:checkbox');

//,를 사용해서 여러 tag들을 동시에 선택
// id가 gil이거나, class가 log이거나 p tag인 요소들 모두 선택
$('#gil, .log, p')
```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[제이쿼리 선택자(jQuery selector)[ZeroCho Blog]](https://www.zerocho.com/category/jQuery/post/57a9a371e4bc011500624ba3)


[]()

[]()
