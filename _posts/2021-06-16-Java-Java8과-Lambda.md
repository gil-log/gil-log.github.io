---
title: "[Java] Java8과 Lambda"
last_modified_at: 2021-06-16T00:27
categories: 
  - modernjavainaction
tags: 
  - 'Java' 
  - 'Java 8' 
  - 'Modern Java In Action' 
  - 'lambda'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[Modern Java in Action](http://www.kyobobook.co.kr/product/detailViewEng.laf?ejkGb=BNT&mallGb=ENG&barcode=9781617293566)

---

# Lambda

**`Java 8`에서는 `기명 메소드(Named Method)`를 `일급 값(일급 시민)`으로 취급**할 뿐 아니라,
**`Lambda` 또는 `익명 함수(Anonymous Functions)`를 포함**하여 **함수도 값으로 취급**할 수 있다.

예를 들면 **`(int x) -> x + 1` 즉 `x`라는 인수로 호출**할 경우 **해당 인수에 1을 더한 값을 반환**하는 **동작을 수행하도록 코드를 구현**할 수 있다.

**기존에 참조할 만한 클래스나 메소드가 없을 경우** 새로운 **`Lambda` 문법을 이용**해 **간결하고 즉각적으로 코드 구현이 가능**한 것이다.

**`Lambda` 문법 형식으로 구현된 Program**을 **`Functional Programming`**,

즉 **Function을 일급 값(일급 시민)으로 넘겨주는 Program을 구현한다 라고 표현**한다.

이를 코드로 살펴보면 아래 예시와 같다.

```java
// Java 8 이전
File[] hiddenFiles = new File(".").listFiles(new FileFilter() {
	public boolean accept(File file) {
    	return file.isHidden();
    }
});

// Java 8 이후
File[] hiddenFiles = new File(".").listFiles(File::isHidden);
```

위 코드 예시 처럼 Java 8 이후 부터는,

**`메소드 참조(::)` 문법을 이용**해 **직접 함수(`isHidden()`)를 메소드(`listFiles()`)로 전달** 할 수 있다.