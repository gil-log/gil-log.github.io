---
title: "[IDE] Eclipse Project Explorer > Editor Area 안에 존재해 소스 오픈 시 같은 영역에서 열리는 현상 해결"
last_modified_at: 2021-04-16T07:34
categories: 
  - ide
tags: 
  - 'eclipse' 
  - 'ide' 
  - '해결'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[이클립스 콘솔창하고 프로젝트창이 같은탭에서 열리는데 원래대로 바꾸려면 어떻게하죠??[OKKY - 귀디님 질문]](https://okky.kr/article/279555?note=946811)

[]()

[]()

[]()

[]()

[]()

<br>


# 문제 상황

![](https://images.velog.io/images/gillog/post/e8ae077f-af9d-4ccc-ad5c-f6b0fea3f072/image.png)


이클립스를 오랜만에 켜서 화면 구성을 하다가, **Project Explorer 창을 옮기다 보니** 아래와 같이 **소스 코드 오픈 시 Project Explorer가 있는 탭에서 열리며 불편한 상황이 발생**했다...


![](https://images.velog.io/images/gillog/post/37e05d6c-bd78-4e3b-a10e-4dbe3cb62168/image.png)


<br>

# 핵심 단어


나는 이번 에러? 해결을 위해 핵심적인 단어로

`eclipse`, `project explorer`, `에디터 영역` 으로 꼽았다.

<br>

# 해결 방법


해결 방법은 아주 간단했다.

Eclipse Tab 중에 **Window > Perspective > Reset Perspective**


![](https://images.velog.io/images/gillog/post/ec3c86ee-928f-4a21-bc63-d26ef2c89707/image.png)


**Reset Perspective 항목을 클릭해 리셋 하면 끝**이었다..






![](https://images.velog.io/images/gillog/post/724b3c5f-211f-4718-9647-6ef62a62527a/image.png)


**다시 편안~~ 하게 소스 코드를 열람** 할 수 있다!

오늘도 해결 완료!