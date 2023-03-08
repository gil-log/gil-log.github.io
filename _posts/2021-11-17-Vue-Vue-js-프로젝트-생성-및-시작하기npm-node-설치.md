---
title: "[Vue] Vue js 프로젝트 생성 및 시작하기(npm, node 설치)"
last_modified_at: 2021-11-17T22:28
categories: 
  - vuejs
tags: 
  - 'Node' 
  - 'cli' 
  - 'npm' 
  - 'vue'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import 
[Vue-시작하기-Vue-설치하기-뷰-시작하기[김통팅 개발 끄적끄적]](https://kimtongting.tistory.com/entry/Vue-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Vue-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-%EB%B7%B0-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0)


---

# Node 설치

**`Vue` Project** 를 위해선 **`Node.js`와 `npm` 설치가 선결**되어야 한다.

먼저 자신의 운영체제에 맞게 `Node.js` 를 설치하자.

- Node Js 링크 >> [https://nodejs.org/ko/]
(https://nodejs.org/ko/)

제대로 설치 됐는지 확인 하기 위해 아래 명령어를 입력해보자.

```
node -v
```

![](https://images.velog.io/images/gillog/post/e02879a1-49ee-4441-ba3f-5ebe95ba0656/image.png)

---

# Vue 설치

그 후 **npm으로 `vue`를 설치**해주자.


```
npm install vue
```

제대로 설치 됐는지 확인 하기 위해 아래 명령어를 입력해보자.

```
npm vue -v
```

![](https://images.velog.io/images/gillog/post/2ea470ca-b5f8-44d8-9840-275af28ab39d/image.png)


## Vue Cli 설치

**Vue 설치 확인 후**에는 **Vue 명령어**와,

**빠른 프로젝트 생성, 관리 라이브러리 `vue-cli`를 설치**하자.



```
npm install -g @vue/cli c
```

![](https://images.velog.io/images/gillog/post/748e2f05-49d6-4d2e-be87-f3a84b657059/image.png)


그 후 **Vue 프로젝트 생성을 위해 아래 명령어로 `cli-init` 역시 설치**하자.

```
npm i -g @vue/cli-init
```

![](https://images.velog.io/images/gillog/post/e7cc388c-c28f-4fba-85dc-1c24d9ea9e05/image.png)


이제 아래 명령어로 **터미널에서 `Vue` 명령어가 정상 동작하는지 확인**해보자.

```
vue -V
```
![](https://images.velog.io/images/gillog/post/fb44c96f-ce9e-43ff-bc07-0e12636dd5ad/image.png)


---

# Vue 프로젝트 생성

이제 **터미널에서 명령어를 통해서 `Vue` 프로젝트를 생성**할 차례다.

**프로젝트 생성을 원하는 경로로 이동** 한 후 **아래 명령어를 입력**하자.

```
vue init webpack 프로젝트명
```

해당 **명령어 실행 이후**에는 **서버 셋팅 관련 선택 창**들이 나온다.

**자신의 프로젝트 성격에 맞게 설정**해주자.

![](https://images.velog.io/images/gillog/post/54a5b751-23ba-482e-acb7-6bbd3d8e195f/image.png)


커피 한잔 하고 오자 시간이 조금 걸린다. ☕️


  
![](https://images.velog.io/images/gillog/post/abd036ec-b4ba-47e4-85f0-dc9d18db7554/image.png)

**커피 한잔의 시간 후 프로젝트 설치가 완료**되었다.

![](https://images.velog.io/images/gillog/post/3e268759-d97b-4daa-a818-b14499b74c2c/image.png)

---

# Vue 프로젝트 실행

이제 **생성한 `Vue` 프로젝트 경로로 이동** 한 후,

**`npm run dev` 명령어로 서버를 실행**해보자.


```
// 생성한 Vue 프로젝트 경로로 이동
cd 프로젝트 경로

// node package manager로 해당 프로젝트 경로에서 프로젝트 실행
npm run dev
```

![](https://images.velog.io/images/gillog/post/f5b26bed-b2ef-46c7-a905-235eeffd1007/image.png)


![](https://images.velog.io/images/gillog/post/a9e45f3c-c739-42d7-ae12-34fe97644bf4/image.png)


**프로젝트 생성이 완료**되었다.

**이제 이것 저것 자신만의 Vue Project를 만들어 나가보자.**