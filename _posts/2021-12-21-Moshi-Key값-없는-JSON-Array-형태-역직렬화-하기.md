---
title: "[Moshi] Key값 없는 JSON Array 형태 역직렬화 하기"
last_modified_at: 2021-12-21T23:40
categories: 
  - java
tags: 
  - 'json array' 
  - 'moshi' 
  - '역직렬화'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[moshi github](https://github.com/square/moshi)

---

**`Moshi`**는 **`Java`나 `Kotlin`에서 `JSON` 형태의 데이터**를,

**Java 객체 형태**로 **역직렬화나 그 반대로 직렬화 용도로 많이 사용하는 Library**이다.

![](https://images.velog.io/images/gillog/post/109eb1b0-2593-4036-b037-a7ad01761b29/image.png)
_이런 형태의 JSON 데이터를_

![](https://images.velog.io/images/gillog/post/20584faf-f5f0-4dc4-a448-c55590a4c5d0/image.png)

_Java Class 에서 Field명을 JSON의 Key 값을 맞춰 주어 역직렬화_

<br>

**오늘은 아래와 같이 JSON Key 값 없이 JSON Array 형태의 JSON 데이터**를,

**Moshi로 역직렬화 하는 방법을 알아보려한다.**

![](https://images.velog.io/images/gillog/post/6d39e51b-1f5c-4d01-86e9-cc736868de18/image.png)
_JSON Key 값이 없는데 역직렬화 객체 Field 명을 어떻게 붙여줘야하지,,,??_
_라는 생각을 1시간전 나처럼 하신 분들을 위해_

---

# 공식문서

**특정 Library를 사용하다 생기는 고민**은 **결국 공식문서를 살펴보는게 가장 빠르고 현명한 방법**이다.

**[Moshi 공식문서](https://github.com/square/moshi)**를 살펴보면 **아래와 같은 JSON Array 형태를 파싱하는 방법도 설명**되어 있다.

![](https://images.velog.io/images/gillog/post/9061c080-4bf0-40db-bf07-4bdda45a13ed/image.png)

**`Moshi`는 Key값이 없는 JSON Array 형태 데이터**의 경우, 

**데이터 내부 형식에 맞춘 Class**의 **`Java Reflect` Type**을,

**매개변수로 하는 `Moshi` Adapter를 제공**한다.


바로 예제 코드와 함께 살펴보자.

---

# 예제

먼저 **아래와 같은 JSON Array 데이터**가 있다고 하자.

```json

{
	"items":[
		{},
		{},
		{},
		{},
		{},
		{
			"image":"https://images.yogiyo.co.kr/image/yogiyo/PARTNER_FR_IMG/%EC%AB%84%EB%A9%B4%EC%A3%BC%EB%8A%94%EC%82%BC%EA%B2%B9%EB%B3%B8%EB%8A%A5/2021-03-30/%EC%A0%9C%ED%9C%B4FR_20210329_%EC%AB%84%EB%A9%B4%EC%A3%BC%EB%8A%94%EC%82%BC%EA%B2%B9%EB%B3%B8%EB%8A%A5_%EC%99%95%EA%B0%88%EB%B9%84%EB%A7%9B%EB%8F%BC%EC%A7%80%EA%B5%AC%EC%9D%B44%EC%9D%B8_1080x640.jpg?width=1080&height=640&quality=70",
			"id":235590842,
			"price":"16900",
			"name":"\uac08\ube44\ub9db\ub3fc\uc9c0\uad6c\uc7744\uc778"
        	}
 	   ],
	  "slug":"photo_menu_items",
	  "name":"Photo Menu Items"
},

{
	"description":"\ub450\ud23c\ud558\uac8c \uc370\uc5b4 \uc2dd\uac10\uc740 \uc0b4\ub9ac\uace0, \uce7c\uc9d1\uc744 \ub0b4\uc5b4 \uc591\ub150\uc740 \uc3d9\uc3d9\ubc34\n\uff081\uc778 1\ubc25, 1\ucc0c\uac1c \uc81c\uacf5\uff09",
	"items":[],
	"image":null,
	"slug":"photo_menu_items",
	"name":"\uac08\ube44\ub9db\ub3fc\uc9c0\uad6c\uc7744\uc778"
}
```
위 JSON Array 데이터 처럼 Key 값으로 데이터가 존재하진 않지만,

**각 Array index**안에서 **`items`, `slug`, `name`, `description`, `image` 형태의 Key 값이 존재**하고,

**`items` 안에는 `image`, `id`, `price`, `name` 형태의 Key 값이 존재**한다.

**이러한 형태를 Java에서 Class 형태로 생성하면 아래**와 같은데,

```java

import lombok.Getter;
import lombok.ToString;

@Getter
@ToString
public class MenuTypeResponse {
    private MenuResponse[] items;
    private String slug;
    private String name;
    private String description;
}


import lombok.Getter;
import lombok.ToString;

@Getter
@ToString
public class MenuResponse {
    private long id;
    private String name;
    private String description;
    private int price;
    private String image;
    private String original_image;
}
```

여기서 **`MenuTypeResponse`는 최 외부 JSON Array의 각 Index 안의 형태를 포매팅한 Class**이고,

**`MenuResponse`는 내부에 `items` keyword의 JSON 형태를 포매팅한 Class**이다.

<br>

이렇게 **JSON Array Index 안의 요소를 Class 형태로 포매팅** 했다면

아래와 같이 **`Moshi`를 생성할 때 `adapter`안**에,

**최종 역직렬화 할 `Collection` Class**와,

**해당 최외부 포매팅 Class**를,

**`Reflect Type` 형태로 선언하여 사용하면 역직렬화** 할 수 있다.

```java
// Java Reflect Type 선언
 Type type = Types.newParameterizedType(List.class, MenuTypeResponse.class);

// Moshi 생성시에 adapter에 해당 Type을 매개변수로 사용
 List<MenuTypeResponse> result = (List<MenuTypeResponse>) new Moshi.Builder()
                .build()
                .adapter(type)
                // 해당 매개변수에는 Json 데이터를 사용
                .fromJson(response.body().source());
```

![](https://images.velog.io/images/gillog/post/93d62367-72d0-47d2-a71c-fbc0723a3a8a/image.png)