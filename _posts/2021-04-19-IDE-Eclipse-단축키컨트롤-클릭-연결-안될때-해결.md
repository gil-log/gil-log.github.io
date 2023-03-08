---
title: "[IDE] Eclipse 단축키(컨트롤) + 클릭 연결 안될때 해결"
last_modified_at: 2021-04-19T00:37
categories: 
  - ide
tags: 
  - 'eclipse' 
  - 'ide' 
  - '클릭 연결' 
  - '해결'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---


# 문제 상황

평소 처럼 코드를 보다가 `Ctrl` + `클릭`으로 해당 클래스나 코드가 있는 곳으로 이동 하려고 했는데, 단축키가 작동하지 않았다.


# 핵심 단어

`Eclipse`, `Ctrl + Click`, `HyperLink`를 핵심 단어로 선택하고 Google에 검색했다.


# 해결 방법

해결 방법은 단순했다.

`Windows` > `Preferences` > `General` > `Editors` > `Text Editors` > `Hyperlinking`으로 이동해서, `Default modifier Key`로 설정된 단축키가 잘못되있는지,

`Open ~~~~ ` 이라고 적힌 `Link Kind`가 정상 체킹 되어있는지 확인하면 된다.

나의 경우는 `Default modifier Key`가 `Ctrl`이 아닌 `Alt`로 설정되어있는 상황이었다.


![](https://images.velog.io/images/gillog/post/56d73488-5354-4ab2-b6c7-95897c5367ab/image.png)

위 사진 처럼 체킹하고 단축키를 `Ctrl`로 설정하면 해결된다.