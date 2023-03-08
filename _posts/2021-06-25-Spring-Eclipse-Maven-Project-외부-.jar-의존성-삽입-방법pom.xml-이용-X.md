---
title: "[Spring] Eclipse Maven Project 외부 .jar 의존성 삽입 방법(pom.xml 이용 X)"
last_modified_at: 2021-06-25T01:51
categories: 
  - ide
tags: 
  - 'Spring' 
  - 'dependency' 
  - 'eclipse' 
  - 'jar' 
  - 'maven'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
**회사 프로젝트에서 외부 인증 `jar` 파일을 받아 적용해야하는 상황**에서 **다음을 위해 길록**해둔다.

## 1. jar 파일들이나 libarary들을 보관할 폴더에 .jar 파일을 옮긴다.

![](https://images.velog.io/images/gillog/post/b2466efc-22c0-4dd9-a758-10ab7eaa4958/image.png)


## 2. Elipse 적용 프로젝트 우클릭 > Properties 클릭

![](https://images.velog.io/images/gillog/post/d4b6654b-f389-4b89-b323-a61350e4986a/image.png)




## 3. `Java Build Path` > `Add External Jars` 클릭

![](https://images.velog.io/images/gillog/post/c0edb09b-e13d-4224-ab5b-851d2c33eda9/image.png)



## 4. 외부 .jar 선택 후 Apply And Close 클릭

**새로 추가한 `.jar`를 확인**하고 **`Apply And Close`** 해준다.

![](https://images.velog.io/images/gillog/post/4fbfd71f-4326-4617-af46-05d69f8a1db6/image.png)

## 5. project > clean 진행


![](https://images.velog.io/images/gillog/post/ed3a30a5-8083-42e2-9931-301dcc203d1f/image.png)

`gil.end()`