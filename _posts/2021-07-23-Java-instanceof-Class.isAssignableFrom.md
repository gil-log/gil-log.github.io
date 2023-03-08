---
title: "[Java] instanceof, Class.isAssignableFrom"
last_modified_at: 2021-07-23T07:56
categories: 
  - java
tags: 
  - 'Class.isAssignableFrom' 
  - 'Java' 
  - 'instanceof'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[instanceof 와 Class.isAssignableFrom 의 차이점](https://jistol.github.io/java/2017/08/22/different-instanceof-isassignablefrom/)


---

# instanceof

**`instanceof`**는 **해당 `Object`**가,

**특정 `Class`, `Interface`**를 **상속하거나 구현 했는지**를,

**`boolean` type으로 return해주는 method**이다.


```java
public class Gillog extends Gil {
	...
}

Gillog obj = new Gillog();

// true
if (obj instanceof Gil) {
	...
}
```

# Class.isAssignableFrom

**`Class.isAssignableFrom`**은 **특정 `Class`**가,

**특정 `Class`, `Interface`**를 **상속하거나 구현 했는지**를,

**`boolean` type으로 return해주는 method**이다.

```java
// true
if (Gillog.class.isAssignableFrom(Gil.class)) {
	...
}
```


즉 **`instanceof`와 `Class.isAssignableFrom`의 차이점**은,

**검사 대상이 `Instance`화 되었는지**이고,
_Memory에 onLoad_

**수행 기능은 같다.**