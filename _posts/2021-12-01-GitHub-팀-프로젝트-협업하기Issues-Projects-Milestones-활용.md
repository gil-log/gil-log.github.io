---
title: "[GitHub] 팀 프로젝트 협업하기(Issues, Projects, Milestones 활용)"
last_modified_at: 2021-12-01T00:48
categories: 
  - 형상관리
tags: 
  - 'Issues' 
  - 'Milestone' 
  - 'github' 
  - 'label' 
  - 'projects'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Team Project

**`Team Project`는 언제나 많은 난관들에 봉착**하게된다.
_**소스코드 통합, 관리, 일정 관리, 업무 분담, 진행 상황 공유, ...**_

**만약 `Git Hub`에서 `Team Project`를 진행 중**이라면,

**`Issues`, `Projects`, `milestones`를 통해서**,

**`Team Project` 협업의 난관들을 해결**해나갈 수 있다.

<br>

오늘은 **`Issues`, `Projects`, `milestones`**를 활용해,

**`Team Project`를 진행하는 방법을 살펴보려한다.**

---

# 시작 전

먼저 **해당 글은 `Git Hub`에서 `Repository`를 생성**해서,

**`Member`들을 초대**해 **`Team Project`를 수행하고 있는 상황을 기본 전제로 시작**한다.

당연하지만 **`Team Project`를 수행하고 있는 `Repository`가 필요**하다!

![](https://images.velog.io/images/gillog/post/786d6f82-1b62-4455-8008-150d9598f66c/image.png)

---
# Issues

**`Repository`의 Tab 가운데 `Issues`는 다양한 방법으로 활용 가능**하다.

![](https://images.velog.io/images/gillog/post/8312a737-a4a3-47c7-a356-19fe5c77f41a/image.png)

**`Issues`에서는** 개발 중인 **기능 관련 이슈**, **문제 상황 등**을,

**`Issues` 생성을 통해 다른 사람들과 공유**할 수 있다.

![](https://images.velog.io/images/gillog/post/50b8ace9-6228-421a-8865-b2bf4879dc39/image.png)

## Issues 생성

**`Issues` 생성**은 **`Repository`의 `Issues` Tab으로 이동**해,

**`New issue` 버튼을 통해 생성**할 수 있다.

![](https://images.velog.io/images/gillog/post/c55f0ce8-e69c-48f9-b26b-4efaeec74832/image.png)

그럼 아래와 같은 화면이 나온다.

![](https://images.velog.io/images/gillog/post/11a39c90-018d-4757-a8fd-2f37ed80eca8/image.png)


## Issues 양식


### Issues 본문

**`Title`에서는 해당 Issue의 제목**,

**Comment 부분에는 자유롭게 해당 Issue에 대한 댓글을 작성**할 수 있다.

![](https://images.velog.io/images/gillog/post/184dfd7c-f597-4667-aec0-f6d2354fd109/image.png)

**`Submit new issue`로 바로 `Issues` 생성을 할 수도 있지만**,

![](https://images.velog.io/images/gillog/post/60ab320f-a890-4425-a903-4d1fa9f17d10/image.png)

**아직 더 활용할 수 있는 요소들이 남아있다.**


### Assignees


먼저 **`Assignees`는 해당 `Issues`에 대해 같이 공유하거나 담당**하게 될,

**`Project Member`를 선택**할 수 있다.

![](https://images.velog.io/images/gillog/post/ab8dae92-8095-40bd-9b5f-7dcf09c147cf/image.png)

### Labels

**`Labels`는 특정 `Issues`를 분류할 수 있는 `Category` 와 같은 요소**이다.

![](https://images.velog.io/images/gillog/post/d277a277-c93e-443e-bd5c-971e05a574a2/image.png)

**`Edit labels`를 통해서 새로 `label`을 생성하거나 편집**할 수 있다.

![](https://images.velog.io/images/gillog/post/16933cfc-8dc9-440a-836a-c9c4d521690b/image.png)

여기서 **생성한 `Label`**을 **`Issue`에 할당**해,

**해당 `Issue`의 성격을 분류**할 수 있다.

### Projects

**해당 `Issue`의 일정 관리를 위해 할당 `Project`를 선택**할 수 있다.

![](https://images.velog.io/images/gillog/post/9b698d1a-8f83-40fc-afbd-9f0c0e175b23/image.png)

**`Project`내에서도 `Issue` 생성이 가능**하며,

**`Project`에 대한 설명은 아래에서 설명**한다.

### Milestone

**`Issue` 생성 요소 중 마지막 요소**로,

**해당 `Issue`의 기한을 선택**할 수 있다.

**`Meilestone`에 대한 설명은 아래에서 설명**한다.

![](https://images.velog.io/images/gillog/post/0a03293f-5829-469d-9a93-55a30509b0a7/image.png)

이제 **`Submit new issue`를 클릭하면 해당 `Issue`가 생성**된다.

![](https://images.velog.io/images/gillog/post/c374ed5a-5727-4de8-8bb7-d0c8b04a358a/image.png)

## Issue 활용

### 의견 공유

이렇게 생성된 **`Issue`에서 서로 자유롭게 `Comment`로 의견을 공유**할 수 있다.

**`@아이디`로 특정 사용자에게 mention 기능도 사용할** 수 있다.
![](https://images.velog.io/images/gillog/post/d074628a-835b-4e8f-bba3-a1c4d7a58efb/image.png)


### Commit 취합

**코드 수정 작업 후 `commit` 명령어로 Version을 남길 때**,

**`[#이슈번호] 커밋내용` 형식으로 메시지**를 남기면,

해당 **`Issue`에서 `Commit` 내역들을 모아 볼수도** 있다.

![](https://images.velog.io/images/gillog/post/e6f45636-db2f-464f-b07e-141bccc833b8/image.png)

**명령어로 아래와 같은 형식**이다.
```bash
git add *
git commit -m "[#이슈번호] 메시지내용"
git push
```
![](https://images.velog.io/images/gillog/post/80a667cb-14c3-4663-b014-e71b277f2d12/image.png)


<br>

**`Issue`를 활용**해서 **만들어야할 기능 관련, 버그 관련, 궁금사항 들**을
**`Label`로 성격을 구분**짓고,
**`Commit` 내역들을 모아 History**를 남기고,
**`Comment`로 서로 의견들을 공유**하자.

![](https://images.velog.io/images/gillog/post/256ccb41-e8ba-4b8e-9e12-d141af5ec37d/image.png)

---

# Projects

**`Projects`로 Project를 세분화해 일정별로 관리**할 수 있다.

**`Repository`의 Tab 중에서 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/b11d166f-3cc8-4e1d-9f95-6e02bf498376/image.png)

아래 처럼 **`Repository`내의 `Project`들을 확인**할 수 있는데,

![](https://images.velog.io/images/gillog/post/2f7695a7-d4a5-4645-85a2-03b0553b7936/image.png)

해당 **Project를 좀더 세분화 시켜 협업하고 싶을 때 사용**하면 된다.


## Proejct 생성

**`Project` 탭에서 `New Project` 버튼을 클릭**해,

**`Project`를 생성**할 수 있다.

![](https://images.velog.io/images/gillog/post/6c2aa4c7-bc00-4b59-a0de-87249ccfc9ef/image.png)

해당 **`Project`의 이름과 설명을 선택**할 수 있고,

**`Template` 부분에서 생성 되자마자 `Template`을 적용시킬지 선택**할 수 있다.



## Project 살펴보기

**`Project`는 기본적으로 `Template`를 선택하여 생성할 수 있는데**,

**`Column`과 `Card`로 구성**되어 있다.

![](https://images.velog.io/images/gillog/post/075a0027-c43e-4776-9913-2e76d789993c/image.png)


## Column

**`Column`은 위 사진에서 `To do`, `In progress`, `Done` 과 같은 요소**이다.

**`Add column` 을 통해 추가**할 수 있다.

![](https://images.velog.io/images/gillog/post/889ffce8-79f9-4f44-b5ce-3e24ffbab11b/image.png)

![](https://images.velog.io/images/gillog/post/2ce2d9a3-d9e2-4035-bc81-e2a162394ad4/image.png)

이렇게 **`Column`들을 단계별로 구분**지어 놓고,

**`Card`를 생성하여 일정을 관리**할 수 있다.


## Card


**`Card`는 `Column` 안에서 사용하는 요소**로,

**실제 작업해야하는 내용들을 만들어 사용**하면 된다.

### Card 생성

**`Project` 화면 우측에 `Add cards`를 통해 생성**하거나,

![](https://images.velog.io/images/gillog/post/31d0c3c3-ebb8-4a06-b1dd-3fdb8b6d4923/image.png)

**`Column` 내에서 `+` 버튼을 클릭해 생성**할 수 있다.

![](https://images.velog.io/images/gillog/post/1b23eecd-dfab-42d2-a82b-4c3cd37f1c01/image.png)


![](https://images.velog.io/images/gillog/post/1cb22f45-6e92-4290-bb59-a7401a94d724/image.png)


### Convert to issue

이렇게 **생성한 `Card`는 단순 제목 부분 밖에** 존재하지 않는다.

**그대로 사용해도 되지만, 이전에 살펴본 `issue`로 전환하여 사용할 수도** 있다.

해당 **`Card`의 `...` 부분을 클릭하고 `Convert to issue` 버튼을 클릭**하면 된다.

![](https://images.velog.io/images/gillog/post/c735b71f-2acb-4ebd-9aa2-66d7a02dba84/image.png)

![](https://images.velog.io/images/gillog/post/975deb0f-0c1e-4a6c-85f8-a81ecd3342d4/image.png)

위 사진 처럼 **`issue`를 생성하며 `Body` 부분을 작성**할 수 있다.

**`project` 내에서 이렇게 생성된 `issue`를 선택**하면,

**우측에 아래와 같이 요약된 `issue`내용을 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/2e4b8e76-47af-4ea4-9364-7181ce9e7b20/image.png)
_여기서도 수정 및 close가 가능._

**`Go to issue for full details` 로 `issue` 화면으로 이동**할 수도 있다.

**`issue`를 생성하고 `Projects` 요소에 `Project`를 선택**하면,

**해당 `Project`내에서도 바로 확인 가능**하다.


## Project 활용

**`Project`를 통해 `issue`들을 세분화 하여 `Project` 단위**로 나누어,

**진행 상황 별로 구분지어 한눈에 관리**할 수 있다.

![](https://images.velog.io/images/gillog/post/0cd9c9a2-59f4-4123-b17b-2f0ef225567a/image.png)

---

# Milestones

**`Milestones`는 `Project`의 `issue`를 기한별로 관리**할 수 있게 해준다.

**`Issue` Tab에서 `Milestones` Tab을 클릭**해보자.

![](https://images.velog.io/images/gillog/post/daea2e01-62c8-42d7-a709-da73fff8c124/image.png)

여기서 **해당 `Repository` 내의 `Milestone`들을 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/5c38b726-f103-4ad4-9e93-607f06dca4c7/image.png)

## Milestone 생성

**`New milestone`버튼을 통해 새로운 `Milestone`을 생성**해보자.

![](https://images.velog.io/images/gillog/post/46bdd2a8-640e-44dc-8f1d-70f2f3c425d7/image.png)

해당 **`Milestone`의 제목, 일정 설명을 설정**할 수 있다.

## Milestone 살펴보기

**`Milestone`을 클릭**하여 살펴보면,

**생성할 때 `Milestone`을 설정한 `issue`들의 목록을 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/536b4ce6-7985-404f-99fa-a8b972a8968e/image.png)


그리고 더 자세히 살펴보면 기한과 달성률을 살펴볼 수 있는데,

![](https://images.velog.io/images/gillog/post/7681aeb0-24b9-4166-aab2-e7c9d5a4a605/image.png)

**이 달성률은 `Open`된 issue와 `Closed`된 issue를 토대로 책정**된다.

**`Milestone`내의 `issue`들이 `Closed`될 때 마다, 달성률이 올라간다.**

![](https://images.velog.io/images/gillog/post/067624f2-2e14-4f95-b1c8-de89e7ae3440/image.png)


이렇게 **`issue`들에 `Milestone`을 할당**해,

**기간별로 `issue`를 관리하며 협업**할 수 있다.


---

오늘은 **`Git Hub`에서 제공하는 `Issue`, `Project`, `Milestone`에 대해** 알아봤다.

**협업의 꽃인 `Issue`**의 **생성과 관리를 어떻게 하느냐에 따라 천차만별의 협업**이 될 것이다.


<br>

**`Project`를 통해서 `Issue`들을 일정에 맞게 세분화하여 관리**하고,

**`Milestone`을 통해 `Issue`들을 기한에 맞게 관리**하며,

**`Commit`들 역시 `Issue`를 통해 취합**하면서

<br>

**`Issue`, `Project`, `Milestone` 활용**으로,

**자신의 Team 상황에 맞는 협업을 알맞게 진행**해보자.