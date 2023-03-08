---
title: "[VSCode] Java JDK Runtime 설정하기"
last_modified_at: 2021-06-02T00:13
categories: 
  - ide
tags: 
  - 'Java' 
  - 'jdk' 
  - 'runtime' 
  - 'vscode'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---


# 🙆‍♂️ import 🙇‍♂️


[VSCODE 환경 SpringBoot 개발환경[Dreamer]](https://parkdream.tistory.com/95)

[]()

[]()

[]()

[]()

<br>

**`VSCode`에서 JDK Runtime 환경 설정**을 해보려한다.

시작하기에 앞서, 본인의 개발환경에 JAVA_HOME 설정을 해주어야 하는데,
**Java 환경 설정이 안되어있다면, [여기](https://velog.io/@gillog/MacOS-MacBook-Java-Home-%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)를 따라가면 된다.** 

_나는 MacOS라 Mac 기준_


---
# JDK Runtime Setting




**Java 환경 설정이 끝났다면, Extension에서 Java Extension Pack**을 설치하자.

![](https://images.velog.io/images/gillog/post/66e27cdd-9d70-4cf6-b48a-60bb078bdefd/image.png)

이제 **`VSCode`에 JDK Runtime Setting**을 해줄것이다.

**`기본설정` > `설정`으로 이동**한다.
![](https://images.velog.io/images/gillog/post/be7d4a65-2a1e-44b1-8f9c-e9cca8417dbd/image.png)

`설정`으로 이동했다면, **`jdk`를 검색한 후 `settings.json에서 편집`을 누른다.**

![](https://images.velog.io/images/gillog/post/ecaeed94-5978-4c96-a09e-f7b3e1fa6ebe/image.png)

**`settings.json`에서 아래 사진 처럼 자신의 JAVA_HOME 경로를 입력**해주면 된다.

![](https://images.velog.io/images/gillog/post/3ae353c8-a580-405e-88ae-00a3936ab6f6/image.png)