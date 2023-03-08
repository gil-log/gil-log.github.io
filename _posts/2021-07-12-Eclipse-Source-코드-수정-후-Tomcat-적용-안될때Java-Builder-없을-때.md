---
title: "[Eclipse] Source 코드 수정 후 Tomcat 적용 안될때(Java Builder 없을 때)"
last_modified_at: 2021-07-12T08:12
categories: 
  - ide
tags: 
  - 'Java Builder' 
  - 'eclipse'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
재택 근무 도중 **집 데스크톱 환경에 회사 프로젝트 환경 셋팅을 하고 개발 도중**,

**Java Source를 아무리 수정해도 maven install로 재 build 하지 않는 이상**,

**수정사항이 적용되지않는 상태**였다.


혹시 **같은 증상을 겪으신분들 중 내가 겪은 케이스의 분들을 위한 글**을 적는다.

내 경우엔 **`Project Properties`에 기본 `Java Builder` 설정이 안되어 있어 Build가 안되고 있었다.**


# 확인해보기

먼저 **해당 `Project` > `Properties`에 들어가본다.**

![](https://images.velog.io/images/gillog/post/e068ed68-e42e-41d3-b1a7-ea9a83dcbd09/image.png)


**`Builders` 에 들어가보면 만약 아래 사진 처럼 `JavaBuilder`가 없다면**, 

**나와 같은 증상을 겪고 있다는 뜻**이다.

![](https://images.velog.io/images/gillog/post/1e944210-f504-42cd-ba65-72d3a77892f2/image.png)

# Java Builder 추가하기


Eclipse Project **`.project` 파일을 열어 아래 내용을 추가**하자.

```
<?xml version="1.0" encoding="UTF-8"?>
<projectDescription>
	<name></name>
	<comment></comment>
	<projects>
	</projects>
	<buildSpec>
		<buildCommand>
			<name>org.eclipse.jdt.core.javabuilder</name>
			<arguments>
			</arguments>
		</buildCommand>
	</buildSpec>
	<natures>
		<nature>org.eclipse.jdt.core.javanature</nature>
	</natures>
</projectDescription>
```
_**이미 사용중인 build 항목이 있으면 저기 `<buildCommnad>`, `<natures>` 부분을 추가**해주면 된다._


이제 다시 **`project` > `Properties`에서 확인**해보자.

![](https://images.velog.io/images/gillog/post/17f34ce8-e16b-44f0-92bf-6da52a336ed5/image.png)

사진과 같이 **`Java Builder`가 보인다면 서둘러 체크하고 `Apply`**해주자.

_이것 때문에 얼마나 고생했는지.... 어후..._