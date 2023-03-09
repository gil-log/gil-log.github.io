---
title: "[Slack] Reminder 설정"
last_modified_at: 2022-01-14T00:21
categories: 
  - cooperation
tags: 
  - 'reminder' 
  - 'slack'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[리마인더 설정 [Slack]](https://slack.com/intl/ko-kr/help/articles/208423427-%EB%A6%AC%EB%A7%88%EC%9D%B8%EB%8D%94-%EC%84%A4%EC%A0%95)


---

# Reminder


**`Slack`은 업무용 메신저로 많은 기업들에서 사용**하고 있다.

**업무를 진행하며 반복적인 특정 작업을 기억하기 쉽게 하기 위해**,

**`Slack`에서는 `Reminder`라는 기능을 제공**한다.

<br>

오늘은 이 **`Reminder`를 설정하는 방법**을 길록하였다.


---

# Reminder 생성

## UI


**`Slack` 특정 채널에 들어가 메시지부분에 `+` 버튼을 클릭**한다.

![](https://images.velog.io/images/gillog/post/7323f307-6eea-400e-9c0f-9dbfe43f60d8/image.png)

아래 처럼 **`리마인더 생성` 버튼을 클릭**한다.

![](https://images.velog.io/images/gillog/post/d8772ba7-fab7-46bf-b032-62baa889a6cc/image.png)

아래 **`언제`, `시간`, `리마인더 내용` 부분을 작성하고 `생성`**을 해준다.

![](https://images.velog.io/images/gillog/post/23022d04-eb31-469e-b1fb-eb6c1a4d6b91/image.png)

## Command

**`Slack` 특정 채널에 들어가 메시지 부분에 아래와 같은 형식으로 명령어를 입력**한다.

```
/remind [@someone or #channel] [what] [when].
```

여기서 **`when` 부분에 시간을 설정하지 않으면 `기본 값`으로 `오전 09시`가 설정**된다.


## 리마인더 예시

|설명|대상|내용|시간|Command|
|:--:|:--:|:--:|:--:|:--:|
|나에게 매일 07시 커피 마시기 리마인드|me|커피마시자|everyday at 07:00am|`/remind me 커피마시자 everyday 07:00`
|`Gil`, `Log`에게 <br>매 주 금요일 10:00시에 보고서 작성 리마인드|`@Gil`, `@Log`|보고서 작성하기|every Friday at 10:00am|`/remind @Gil @Log 보고서 작성하기 every Friday at 10:00am`|
|pj_gillog 채널의 모든 사람에게 매달 25일에<br>월급날 알려주기 리마인드|#pj_gillog|월급날이네용:)|on the 25th of every month|`/remind #pj_gillog 월급날이네용:) on the 25th of every month`|


## 시간 예제

|설명|예시|
|:--:|:------------:|
|at 13:00 tomorrow|내일 13시|
|at 01:00pm|오늘 오후 1시|
|in 30 minutes| 30분 뒤에|
|on October 10th at 08:00am|10월 10일 오전 08시에|
|every Firday|매주 금요일 오전 09시에<br>_시간 미설정 시 기본 값 오전 09:00 설정_|
|on 28 Feb|2월 28일 오전 09시에<br>_시간 미설정 시 기본 값 오전 09:00 설정_|
|at noon on july 7| 7월 7일 정오에<br>_시간 미설정 시 기본 값 오전 09:00 설정_|



---


# Reminder 조회

**`/remind list` 명령으로 해당 `Channel`에 설정된 `Reminder` 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/467bacf3-5909-4ca6-9e83-960d0773eaa7/image.png)

## Reminder 삭제

**`/remind list` 명령으로 `Reminder` 조회 후 우측에 `삭제` 버튼을 통해 삭제**할 수 있다.


---

# Reminder 도움말

**`/remind help`로 `Reminder`관련 도움말 정보를 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/f4d6c06e-c780-416e-a3ee-ebb0746a7043/image.png)
