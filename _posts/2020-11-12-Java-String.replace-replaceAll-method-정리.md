---
title: "[Java] String.replace(), replaceAll() method 정리"
last_modified_at: 2020-11-12T13:28
categories: 
  - java
tags: 
  - 'Java' 
  - 'String' 
  - 'replace'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# replace()


문자열에 지정한 문자" "가 있으면 새로 지정한 문자" "로 바꿔서 출력한다.

```java
String str = "abcdeababcssababcabcddeee"; 
str.replace("abc", "길");
```

- 실행 결과

```
길deab길ssab길길ddeee
```


# replaceAll()


정규표현식을 지정한 문자로 바꿔서 출력한다.

```java
String str = "abcdeababbssababcabcddeee"; 
str.replaceAll("[abc]", "길");

```

- 실행 결과

```
길길길de길길길길길ss길길길길길길길길ddeee
```




<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[JAVA String 클래스 메소드 정리](http://www.dreamy.pe.kr/zbxe/CodeClip/3766960)

  [[Java] String.replace 와 replaceAll 차이점[iseunghan]](https://iseunghan.tistory.com/41)

[]()

[]()

[]()

[]()