---
title: "[VSCode] lombok 적용 안될때, lombok 세팅"
last_modified_at: 2021-06-08T23:24
categories: 
  - ide
tags: 
  - 'LomBok' 
  - 'vscode'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Spring Boot]Visual Studio Code에서 개발하기 2 - lombok 적용하기[자몽이랑꼬부기]](https://hellowk1.blogspot.com/2020/10/spring-bootvisual-studio-code-2-lombok.html)

[]()


<br>

---

아래 사진처럼 **VSCode 에서 lombok `import`나 `Annotation` 사용도 가능**한데,

**선언한 method들을 사용 못하는 경우**가 있다.

![](https://images.velog.io/images/gillog/post/a198a36e-8db2-484d-aab4-419e730a73c6/image.png)

![](https://images.velog.io/images/gillog/post/0ee0b209-9e51-472b-baa9-9ab8fc3303bc/image.png)

원인은 spring boot 등에서 maven 같은(`pom.xml`) **의존성 관리 도구에서 dependency로 lombok이 추가**가 되었지만, 

**VSCode의 lombok 플러그인을 설치해줘야 하기 때문**이다.


이곳 >> [marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=GabrielBB.vscode-lombok) 에서 `Install`을 하거나


![](https://images.velog.io/images/gillog/post/04b1a9d6-ec44-4bce-af61-ca2b39683631/image.png)

**VSCode Market Place에 `lombok`을 검색하고 설치** 해주자.

![](https://images.velog.io/images/gillog/post/e1fcd36b-ac17-4d34-bf34-b4af756da2fe/image.png)

설치 후에 아래 알림창이 뜨면, **`Restart Now`를 클릭**해주자.
![](https://images.velog.io/images/gillog/post/3d1f8211-3394-4c65-8171-d01cd7f9930f/image.png)

그러면 이제 정상적으로 lombok 사용이 가능해진다! 🙆🏻‍♂️

![](https://images.velog.io/images/gillog/post/9f68ae25-601f-4327-9470-44a532456298/image.png)