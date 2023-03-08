---
title: "[Moshi] java.util.Date Type 역직렬화 Error 해결 (Rfc3339DateJsonAdapter)"
last_modified_at: 2021-08-30T22:29
categories: 
  - java
tags: 
  - 'Java' 
  - 'Rfc3339DateJsonAdapter' 
  - 'moshi'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# # import
[How to send Date object through Moshi JSON serializer?](https://stackoverflow.com/questions/39724946/how-to-send-date-object-through-moshi-json-serializer)

# 에러 상황

아래와 같은 **API의 Response를 `Moshi`로 역직렬화 하려는 과정**에서,

**아래 에러가 발생**했다.

![](https://images.velog.io/images/gillog/post/f18d38bb-99af-4ff5-8bff-c91fc3f03962/image.png)



```
java.lang.IllegalArgumentException: 
Platform class java.util.Date requires 
explicit JsonAdapter to be registered at 
com.squareup.moshi.ClassJsonAdapter$1
.create(ClassJsonAdapter.java:65) ~[moshi-1.9.2.jar:na]

```

역직렬화를 수행하던 코드는 아래와 같고,

![](https://images.velog.io/images/gillog/post/5b11b74c-84a0-416d-a41a-5d2f224b3191/image.png)

객체로 담을 Class는 아래와 같았다.

![](https://images.velog.io/images/gillog/post/1f1de427-0c1d-4ff9-b767-a4f36350ab11/image.png)


# 핵심

원인은 **Moshi Adapter를 Build** 해줄때,

![](https://images.velog.io/images/gillog/post/2ebd6ee8-f011-4806-aaff-b6b436ba7819/image.png)

위와 같은 **`RFC 3339` Format의 Date type Adapter를 추가해주지 않아 발생**했다.


# 해결

아래와 같이 **`Moshi` Adapter를 `build()` 하기 전**에

**`RFC 3339` Date Formatter Adapter**,

**`Rfc3339DateJsonAdapter`를 추가해주며 해결** 했다.
```java
KaKaoResponse kakaoUserInfoResponse = 
		new Moshi.Builder()
                         .add(Date.class, new Rfc3339DateJsonAdapter())
                         .build()
                         .adapter(KaKaoResponse.class)
                         .fromJson(response.body()
                         .source());
```

![](https://images.velog.io/images/gillog/post/b4a657f1-468d-4435-ac3f-ef1a2bbf6186/image.png)