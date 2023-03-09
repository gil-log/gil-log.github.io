---
title: "[GitHub] Organization 계정 생성"
last_modified_at: 2021-11-25T23:34
categories: 
  - configuration-management
tags: 
  - 'github' 
  - 'organization'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[Creating a new organization from scratch[docs.github.com]](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/creating-a-new-organization-from-scratch)
[GitHub - Organization 관리하기[git-scm.com]](https://git-scm.com/book/ko/v2/GitHub-Organization-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0)


---

# Organization 계정

**`Organization` 계정**은 **`GitHub`에서 개인 계정 이외의 협업 관리 단체 계정**이다.

**개인 계정 처럼 `Organization` 계정 역시 Project Namespace**이지만,

**여러 명**이 **같은 Project를 관리하는데 사용하는 그룹 계정**이다.

**팀원들을 Sub Group 으로 나누어 관리하는 Tool도 제공**한다.

## Why Using???

**개인 계정으로도 충분히 `Repository`를 생성**해서 **다른 개인 계정을 `invite`해서 협업**할 수 있다.


![](https://images.velog.io/images/gillog/post/5c2e1020-ba33-4b86-8b8c-72af2692ac15/image.png)


`Organization` 계정과 개인 계정의 엄청난 기능적 차이가 존재하지 않지만,

**개인 계정으로 협업**할 경우 **해당 `Repository`를 소유 계정 한명에게 의존하는 형태로 협업** 하는것이 **가장 큰 단점**이다.

![](https://images.velog.io/images/gillog/post/5a29f189-cfda-4c97-b3e0-b3bdbd9da720/image.png)

_내 맘대로 추방하고 추가할 거야~ 😈😈_
_이런일은 없겠지만.._

또한 **개인 계정으로 관리**할 경우, **`Repository` 권한을 한번에 관리**하거나,

**다수의 `Repository` 관리자를 지정하는 것도 불가능**하다.

<br>

**이러한 단점을 보완하고 싶다면, `Organization` 계정을 사용해보자.**

<br>

**`Organization` 계정을 생성**해 **특정 단체가 속한 `Repository`를 생성**할 수 있고,

**단체에 속한 개인 계정들을 관리**할 수도 있다.

또한 **협업을 위한 다양한 기능들도 제공**한다.

---

# Organization 계정 생성

이제 **아래 과정을 따라서 `Organization` 계정을 생성**해보자.


## GitHub 개인 계정 로그인

**`Organization` 계정 생성에는 개인 계정도 필요**하다.

**`Organization` 계정을 생성한 개인 계정이 자동으로 관리자**가 된다.

**먼저 GitHub에 개인 계정으로 로그인**한 후,

**우측 상단의 `+` 기호 밑의 드롭다운을 클릭**해,

**`New organization` 을 클릭**하자.

![](https://images.velog.io/images/gillog/post/bdba371f-9394-49d4-975e-1c53d678f057/image.png)

## Plan 선택

**이제 Plan을 설정**할 수 있는데,

**`Free`, `Team`, `Enterprise`를 선택**할 수 있다.

![](https://images.velog.io/images/gillog/post/aa322e98-0166-4248-82c0-4f2b82ba0cee/image.png)


**자신의 단체 성격에 맞게 Plan 별 기능을 비교해보고 선택**하자.
_본인은 `Free` 계정으로 생성_


## Organization 설정

이제 **자신의 `Organization`에 맞게 단체명, 대표이메일, 단체성격을 설정**하고,

**간단한 인간 증명 퍼즐? 을 통해 다음 단계로 진행**하자.



![](https://images.velog.io/images/gillog/post/e6453250-b1ae-4d3d-94b4-1f8a51f24134/image.png)

## Organization 초대

이제 **해당 `Organization`이 생성**되었고 **다른 개인 계정을 초대**할 수 있다.

**지금 초대하지 않아도 언제든 초대할 수 있으니**,

**`Complete setup`으로 설정을 마칠 수 있다.**

![](https://images.velog.io/images/gillog/post/9af16568-bf16-4082-8563-55b7abdea77f/image.png)


## 설문조사

**마지막으로 `Github`에 유용한 정보를 제공하기위해 설문조사에 참여할 수 있다.**

![](https://images.velog.io/images/gillog/post/1f7e8bc4-f421-49c1-bee9-7ef0075fd79c/image.png)



**설문에 참여하거나 `Submit`을 눌러 Skip**하자.
_나는 바로 Skip zzz_

![](https://images.velog.io/images/gillog/post/d08dc894-a0e0-4fb0-96e0-d7f51c823892/image.png)



## 완료

`Organization`이 생성되었다.

![](https://images.velog.io/images/gillog/post/4c282216-bb69-452e-a4fc-78aed5960722/image.png)

이제 **해당 `Organization`에 속한 Member들**은,

**자신의 Github Profile에 `Organization`이 노출**된다.

![](https://images.velog.io/images/gillog/post/2f80cbf6-ec11-4139-bb2b-ea15d3a4de11/image.png)




**다음번엔 `Organization` 기능들에 대해 더 알아보고 이제 글을 마친다.🙋🏻‍♂️**
