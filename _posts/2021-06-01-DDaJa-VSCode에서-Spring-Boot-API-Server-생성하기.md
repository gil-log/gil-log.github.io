---
title: "[DDaJa] VSCode에서 Spring Boot API Server 생성하기"
last_modified_at: 2021-06-01T23:58
categories: 
  - sideproject
tags: 
  - 'DDaJa' 
  - 'SIDEPROJECT' 
  - 'Springboot' 
  - 'api server' 
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

**기존에 먼저 작업 중이던 `Vue.js` FrontEnd Server는 `VSCode`에서 작업 중**이었다.

**이제 BackEnd API를 담당해줄 API Server를 기본 틀 구축을 진행**하려 하는데, **사실 `Eclipse`나 `STS(어차피 Eclipse 기반)`이나 `InteliJ`나 이런 IDE**를 써도 되지만,
**기존 `FrontEnd Server`를 `VSCode`에서 작업**하고 있었으니까, 이번 사이드 프로젝트에서 **BackEnd Server는 `VSCode`를 통해서 구축하고, 작업** 해보려 한다.

`VSCode`에서`Spring Boot` Server 구축을 위해 먼저 Java 환경 설정을 진행 해주어야 한다.

혹시나 **Java 환경 설정이 안되어있다면, [여기](https://velog.io/@gillog/MacOS-MacBook-Java-Home-%ED%99%98%EA%B2%BD-%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)를 따라가면 된다.**
_나는 MacOS라 Mac 기준_

그 후 **`VSCode`에 JDK Runtime 설정이 안되어 있다면, [여기](https://velog.io/@gillog/VSCode-Java-JDK-Runtime-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)를 따라가면 된다.**


---



# Spring Boot Init


이제 **`Extenstion`에서 `Spring Boot Extension`을 설치**한다.

![](https://images.velog.io/images/gillog/post/01bc2d7b-1526-4643-b657-d7d01c686ad6/image.png)


그 후 **`Shift` + `Commnad` + `P`를 입력해 `VSCode` Command창**을 띄우고,
**`spring maven`을 검색**해 **`Spring Initalizr: Create a Maven Project...`을 선택**한다.

![](https://images.velog.io/images/gillog/post/1be4eb60-13c8-4586-8a34-a7f47eb56763/image.png)


이제 아래 사진 처럼, **자신의 프로젝트에 맞는 `GroupID`, `Artifact ID`, `Spring Boot Version`, `Java Version`, `Packaging Type`등을 선택**해 준다.

![](https://images.velog.io/images/gillog/post/b59d4155-ef4f-4ca2-be07-fbc354ae3df5/image.png)

![](https://images.velog.io/images/gillog/post/35feafd9-2f62-4f62-8726-2298313ab329/image.png)

![](https://images.velog.io/images/gillog/post/3dc9a6ab-f949-4054-be33-0d8483222f14/image.png)

![](https://images.velog.io/images/gillog/post/016f3b39-2d88-41b6-a845-35eaaaee7fb7/image.png)

![](https://images.velog.io/images/gillog/post/623fbef5-5439-4ff1-b61f-9e385bf47b3e/image.png)

그 후, **자신의 프로젝트에 맞는 `Dependency`를 입맛대로 선택**해주면 된다.

_이게 진짜 Spring Boot의 힘인것 같다... 환경 구축이 너무 편한다.....🤭😎_

![](https://images.velog.io/images/gillog/post/256b099d-f8a8-440d-89d1-ae7eac1c09ff/image.png)

그 후 **Spring Boot Project를 저장할 경로를 설정**해주면 구축이 끝난다...
![](https://images.velog.io/images/gillog/post/913a21be-7301-4e36-bdd5-9e5904410494/image.png)
_So EZ. Spring Boot JJang JJang. :) 👍🏻_