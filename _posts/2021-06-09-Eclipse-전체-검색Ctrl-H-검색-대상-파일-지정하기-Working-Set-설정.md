---
title: "[Eclipse] 전체 검색(Ctrl + H) 검색 대상 파일 지정하기, Working Set 설정"
last_modified_at: 2021-06-09T02:08
categories: 
  - ide
tags: 
  - 'Working set' 
  - 'eclipse' 
  - '전체 검색'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

**`Eclipse`에서 전체 검색(`Ctrl + H`) 로 검색**을 할 때, 
**검색을 원치 안하는 파일들이 포함되어 검색 결과를 늘리는 경우가 종종 발생**한다.

>jQuery.min.js, 오픈소스라이브러리.js, 템플릿.css 뭐 등등,,


이때 전체 검색 대상이 되는 범위 Scope를 설정할 수 있다.


# Eclipse 검색 Scope 설정

**먼저 `Ctrl + H` 로 `Eclipse` 검색 창**을 띄어보자.


![](https://images.velog.io/images/gillog/post/352c12dd-ded2-4e37-8c78-77530afdd8af/image.png)


**검색 창 밑에 `Scope` 에서 `Working set`을 설정**해 주면 된다.
_**원하는 검색 범위 에서만 검색**_


![](https://images.velog.io/images/gillog/post/5e2fd8ef-0295-48fc-a7ff-ae70d869cf11/image.png)

위 사진 처럼 **원하는 검색 대상 경로들만 선택해서 하나의 `Working Sets`을 구성**할 수 있다.

**`Select Working Sets` 창에서 최초 아무 Working Set도 없다면**,
**`Add`를 통해서 원하는 프로젝트의 검색 대상에 포함될 경로들을 설정해주고 `finish`를 선택**하면 된다.

<br>

**여러가지 `Working Sets` 을 만들어서 검색 대상을 지정**할 수도 있고,
**다시 프로젝트내에서 전체 검색**을 하고 싶으면 단순히 **`Scope`에서 `Workspace`로 선택해주면 간편히 다시 변경**할 수도 있다.

![](https://images.velog.io/images/gillog/post/dd9e738f-8591-4785-82a2-781c5f39af6a/image.png)