---
title: "[js] FormData()"
last_modified_at: 2021-06-17T00:41
categories: 
  - javascript
tags: 
  - 'JavaScript' 
  - 'formData'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[FormData()[MDN Web Docs]](https://developer.mozilla.org/ko/docs/Web/API/FormData/FormData)

---

# FormData

```javascript
var gilFormData = new FormData(document.getElementById('formId'));
```


**`FormData`는 Parameter로 HTML의 `<form>` 태그 요소의 `id`**를 받으면,
**해당 `form`의 현재 `key`와 `value`들로 Data**가 채워진다.

**`key` 값**으로는 **해당 `form`을 `submit`한 요소들**의 **`name`을 사용**한다.

**`value` 값**으로는 **해당 `form`을 `submit`한 요소들**의 **`value`를 사용**한다.


보통 아래와 같은 형식으로 사용된다.

```javascript
<form id="gilForm" name="gilForm">
  <div>
    <input type="text" name="name" value="gil">
  </div>
  <div>
    <input type="text" name="acc" value="seoul">
  </div>
  <div>
    <input type="file" name="file">
  </div>
  <input type="submit" value="Submit!">
</form>

var gilForm = document.getElementById('formId');
var gilFormData = new FormData(gilForm);
```

# Methods

## .set()

**`.set()`**은 **`FormData` 객체내에 기존 `Key`에 새로운 `Value`를 설정**하거나,
**`Key`가 없는 경우 새로운 `Key`와 함께 `Value`를 추가**한다.

**`FormData.append()`와 다른 점**은 **`.set()`은 기존 `Key`가 존재**할 경우 **모든 기존 `Value`를 새로운 `Value`로 덮어씌우지만,**

**`.append()`는 기존 `Value` 집합의 끝에 새로운 `Value`를 추가**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.set("newKey", "1");
gilFormData.set("alreadyKey", "11");
gilFormData.set("alreadyKey", "22");

// gilFormData.get("newKey") == 1
// gilFormData.get("alreadyKey") == 22
```

### parameter

`.set()`는 parameter로 **`name`, `value`, `_filename`**을 받을 수 있다.

**`name`, `value`는 각각 `Key`, `Value`로 사용**된다.

**`filename`은 `Optional`**로 **`file` 전달 시 `File` 객체의 기본 파일 이름을 지정하는 용도로 사용**된다.
_`Blob`객체의 기본 파일 이름은 `blob`_

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.set('gilFile', gilFileInput.files[0], 'gil.png');
```



## .append()

**`.append()`는 기존 `FormData` 객체 기존 `Key`에 새로운 `Value`를 추가**하거나, 
**`Key`가 없는 경우 새로운 `Key`와 함께 `Value`를 추가**한다.

**`FormData.set()` 과 다른 점**은 **`.set()`은 기존 `Key`가 존재**할 경우 **모든 기존 `Value`를 새로운 `Value`로 덮어씌우지만,**

**`.append()`는 기존 `Value` 집합의 끝에 새로운 `Value`를 추가**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append("newKey", "1");
gilFormData.append("alreadyKey", "11");
gilFormData.append("alreadyKey", "22");

// gilFormData.get("newKey") == 1
// gilFormData.get("alreadyKey") == ['11', '22']
```
### parameter

`.append()`는 parameter로 **`name`, `value`, `_filename`**을 받을 수 있다.

**`name`, `value`는 각각 `Key`, `Value`로 사용**된다.

**`filename`은 `Optional`**로 **`file` 전달 시 `File` 객체의 기본 파일 이름을 지정하는 용도로 사용**된다.
_`Blob`객체의 기본 파일 이름은 `blob`_
```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gilFile', gilFileInput.files[0], 'gil.png');
```


## .delete()

**`.delete()`**는 **`FormData` 객체에서 `Key`값을 통해 `Key`와 `Value`를 삭제**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.delete('keyName');
```

## .entries()

**`.entries()`**는 **해당 `FormData` 객체에 포함된 모든 `Key`, `Value` 쌍을 `iterator`로 반환**한다.

**각 쌍의 `Key`는 `USVString Type`**이고, **`Value`는 `USVString Type` 이거나 `Blob`**이다.
_**`USVString`**은 **유니코드 스칼라 값의 모든 가능한 시퀀스 집합**_
_**JavaScript로 전달**될 땐 **`String`으로 Mapping** 됨_


```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gil1', 'log1');
gilFormData.append('gillog1', 'loggil1');

for(var pair of formData.entries()) {
   console.log(pair[0]+ ', '+ pair[1]);
}
// gil1, log1
// gillog1, loggil1
```

## .get()

**`.get()`**는 **`FormData`에 포함된 `Key`값의 `Value`를 반환**한다.

만약 **해당 `Key`에 `Value`가 2개 이상**이라면, **첫 번째 `Value`만 반환**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gillog', 'useful');
gilFormData.append('gillog', 'good');

gilFormData.get('gillog');
// useful
```

## .getAll()

**`.getAll()`**은 **`FormData` 객체에 포함된 지정된 `Key`와 관련된 모든 `Value`를 반환**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gillog', 'useful');
gilFormData.append('gillog', 'good');

gilFormData.getAll('gillog');
// ['useful', 'good']
```


## .has()

**`.has()`**는 **`FormData`에 특정 `Key`가 포함되어있는지 여부를 `boolean`으로 반환**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gil', 'useful');
gilFormData.append('log', 'good');

// true
gilFormData.has('gil');

// false
gilFormData.has('gillog');
```

## .keys()

**`.keys()`**는 **`FormData`에 포함된 모든 `Key`의 `Iterator`를 반환**한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gil', 'useful');
gilFormData.append('log', 'good');
gilFormData.append('gillog', 'so useful and good');

for (var key of gilFormData.keys()) {
   console.log(key);
}

// gil
// log
// gillog
```

## .values()

**`.values()`**는 **`FormData`에 포함된 모든 `Value`의 `Iterator`를 반환** 한다.

```javascript
var gilFormData = new FormData(document.getElementById('formId'));

gilFormData.append('gil', 'useful');
gilFormData.append('log', 'good');
gilFormData.append('gillog', 'so useful and good');

for (var key of gilFormData.values()) {
   console.log(key);
}

// useful
// good
// so useful and good
```