---
title: "[GitHub] Organization Team 관리"
last_modified_at: 2021-11-30T00:26
categories: 
  - configuration-management
tags: 
  - 'github' 
  - 'organization' 
  - 'team'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[GitHub - Organization 관리하기[git-scm.com]](https://git-scm.com/book/ko/v2/GitHub-Organization-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)

---

# Team

**`GitHub`에서 `Organization`을 생성**하여 **다른 개인 계정을 초대**했다면,

이제 **`Organization`에서 해당 개인 계정들을 어떻게 관리**하는지 알아보려한다.

<br>

**`Organization`에서 `개인(Member)`과 `Repository`는 `Team`을 통해 연결**된다.

**`Organization`의 `개인(Member)`과 `Repository`**는 **`Team`으로 관리**되고,

**`Repository`에 대한 권한 설정도 `Team`을 통해** 이루어진다.

<br>

만약 **특정 `Organization`**에 **`FrontEnd`, `BackEnd`, `DeployScript`**,

**세 가지 `Repository`가 있다고 가정**하자.

**`FrontEnd`에 해당하는 `Repository`**에는 **`HTML/CSS/Script` 소스 코드가 저장**되어 있다면,

**해당 소스 코드를 작업**하는 **`FrontEnd Developer`(개인 계정)**은,

**해당 `Repository`에 접근 권한이 설정되어 있어야 한다.**

<br>

마찬가지로 `BackEnd`에 해당하는 `Repository`에는 해당 소스 코드를 작업하는,

개인 계정이 접근 권한을 가지고 있어야 한다.

<br>

**`Organization`에서는 이러한 행위를 `Team`을 통해서 관리**한다.



---

# Team 생성

그러면 **`Team`을 생성**해보자.

먼저 **`Organization` 페이지에 접속**한다.

**이때 해당 `Organization`의 관리자 개인 계정으로 접속**해야 한다.

<br>

이제 **`Tab` 목록 중에서 `Team`을 클릭**해보자.


![](https://images.velog.io/images/gillog/post/21e3913f-1521-49d6-987b-182ee06b8687/image.png)

**아래와 같은 화면에서 `New team`을 클릭**해보자.

![](https://images.velog.io/images/gillog/post/76213168-e18a-4a4a-9226-730f550c748b/image.png)

## Team 설정

이제 **아래와 같은 화면에서 생성할 `Team`에 대한 설정**을 할 수 있다.

![](https://images.velog.io/images/gillog/post/efff7655-78a6-4a11-b61f-851221fe42dd/image.png)

**`Team name`, `Description` 에서는 해당 Team의 이름과 설명**에 대해 작성할 수 있다.

**`Parent team`**의 경우 **해당 `Organization`에 다른 `Team`이 존재**한다면,

**생성할 `Team`이 포함될 부모 `Team`을 설정**할 수 있다.

**[Ex] Developer Team에 포함되는 BackEnd, FrontEnd Team**
- `Developer`(Parent Team) 
  - `BackEnd`(Child Team)
  - `FrontEnd`(Child Team)

<br>

**`Team visibility`**는 **해당 `Team`**이 **`Organization`에 속한 다른 `Member`들 에게**,

**노출** 및 `@mentioned` 로 **`mention(언급 기능)`이 가능하게 할 건지 설정**할 수 있다.


해당 **`Team` 성격에 맞게 설정한 후 `Create team`을 클릭**하자.


![](https://images.velog.io/images/gillog/post/305d12a5-9fa7-4c21-a98f-f5d143197586/image.png)


<br>

**본인은 사이드 프로젝트 용도 `Organization`**이라 **관리자 권한 Team(`Master`)**과,

**일반 개발 권한 Team(`Developer`) 두 가지로 구성**하였다.


**`Developer` `Team`은 `Master` `Team`의 자식 `Team`으로 생성**하였다.

![](https://images.velog.io/images/gillog/post/9f51be2c-316b-459a-b330-81f103c06ca3/image.png)


![](https://images.velog.io/images/gillog/post/96a8bcbc-ebc8-4ad7-a514-3084e6fec441/image.png)
_코드리뷰, 조직관리 용도 Team Master_

생성된 **`Team` 화면에서 해당 `Team`에 소속될 `개인(Member)` 계정을 추가**할 수 있다.

![](https://images.velog.io/images/gillog/post/283b3926-3b2a-4d35-b16c-de9a59f43e1e/image.png)
_개발 참여 용도 Master Team의 자식 Team Developer_

<br>

아래와 같은 구성이다.

![](https://images.velog.io/images/gillog/post/35483395-b8d7-4dd9-b10d-d4949bef0600/image.png)

이제 **`Git Hub` `Organization`을 `Team`으로 관리**해보자.

---

# Repository Team 할당

이제 위 과정에서 **생성한 `Team`을 해당 `Organization`의 `Repository`에 할당**해보자.

먼저 **`Organization`에 `Repository`를 생성**하자.
_**일반 계정에서 `Repository` 생성과정과 동일**_

<br>

그 후 **해당 `Repository`에 접속**한다.


![](https://images.velog.io/images/gillog/post/81323924-7377-43e6-84a3-d06e5b0558a0/image.png)

그 후 **`Settings` Tab에서 `Manage access` 를 선택**하면,

**해당 `Repository`에 접근 가능한 `Member`, `Team`을 설정**할 수 있다.
![](https://images.velog.io/images/gillog/post/a0501787-4fd9-4455-ae25-a517b5e374ef/image.png)

여기서 **`Add teams`를 선택**하자.

![](https://images.velog.io/images/gillog/post/3015a67c-bef7-42fc-854a-7fa353cdddbb/image.png)

그럼 이제 **해당 `Repository`에 해당 `Team`의 권한을 설정**할 수 있다.


## Repository 권한 설정

**`Repository`에 `Team`을 할당**하면 해당 **`Team`의 권한을 설정**할 수 있다.

**아래 권한 목록을 살펴보고 알맞게 설정**하자.


- `Read`
  - 단순 읽기, 토론 참여 가능
  
- `Triage`
  - 쓰기 권한은 없지만 이슈와 Pull Requests에 참여 가능
  
- `Write`
  - Project에 소스 코드를 Push 하는 등 쓰기 권한 보유
  
- `Maintain`
  - 민감한 Action등에 권한은 없지만 해당 Repository를 전반적으로 관리 가능
  
- `Admin`
  - 모든 권한 보유
  
![](https://images.velog.io/images/gillog/post/f3605b99-ac0e-47bf-b61d-d8f6c8514708/image.png)

이렇게 **해당 `Repository`에 접근 가능한 `Team`과 그 권한을 설정**할 수 있다.
