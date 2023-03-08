---
title: "[Window] telnet 명령어 설정 및 port ping 확인 하기"
last_modified_at: 2022-02-10T04:26
categories: 
  - windowos
tags: 
  - 'Telnet' 
  - 'window'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

---

# telnet

**`TELNET`**은 **원격환경 등**의 **Network 상에서 다른 Server나 컴퓨터로 접속을 도와주는 서비스를 지칭**한다.

흔히 **`Windows`에서는 `telnet`을 통해**서 **다른 Server로의 Ping 테스트나 Port**로,

**데이터 송수신이 원활히 일어나는지 확인하는 용도로 사용**한다.


<br>

흔히 **`ping -t ???` 형식**으로 **간단히 해당 주소로 데이터 송수신 테스트를 해볼 수 있지만**,

**특정 Port 등을 통해서만 접속이 허용되는 내부망 환경 등**에서는,

**특정 Port로의 데이터 송수신 테스트가 필요**하다.

![](https://images.velog.io/images/gillog/post/263787df-5e27-450c-b1b1-94a36d8fd902/image.png)


<br>

그래서 **Window 환경에서 특정 주소의 Port에 대한 테스트**를 위해,

**`telnet` 명령어를 사용**해야 하는데,

**`telnet` 명령어 활성 방법**과 **명령어를 통해 Port 테스트를 하는 방법**을 길록한다.


---

# telnet 명령어 활성

**`cmd` 창을 열고 바로 `telnet` 명령어**를 치면,

**사용할 수 없는 명령어라고** 나오는데,

![](https://images.velog.io/images/gillog/post/92df6c9e-5f66-43f9-8e47-a29bf46e3ee1/image.png)

**`telnet` 명령어 활성**을 위해서는,

**특별한 설치과정 없이 간단한 설정 체크만** 해주면 된다.

<br>



**`Windows 7` 이상 O/S를 기준**으로

**`제어판` > `프로그램` > `프로그램 및 기능` 으로 이동**하자.

![](https://images.velog.io/images/gillog/post/fd82c0ed-fe66-442a-9339-2df52a59be11/image.png)

**여기서 `Windows 기능 켜기/끄기` 를 선택**해주자.


그럼 아래와 같은 창이 나오는데,

여기서 **`텔넷 클라이언트` 부분을 체크**해주자.

![](https://images.velog.io/images/gillog/post/6519110e-12ee-4e2e-ac11-23450ec752ea/image.png)


**간단한 설정 완료 창이 나온 후**,

이제 cmd 창으로 돌아가 **`telnet` 명령어를 사용할 수 있다.**


---

# telnet Port test

**`telnet` 명령어의 형식은 아래**와 같다.

```cmd
telnet [-a][-e 이스케이프 문자][-f 로그 파일][-l 사용자][-t 터미널][호스트 [포트]]
```

**명령어에 사용 가능한 각 옵션이나 인자에 대한 설명**은 아래와 같다.

|인자|설명|
|:--:|:--:|
|-a|자동 로그온을 시도, `-l` 옵션과 동일하지만,<br>현재 로그온된 사용자 이름을 사용
|-l|로그온을 시도, 사용자 이름을 변수로 사용자 이름 지정 가능, <br>원격 시스템에서 `TELNET ENVIRON` 옵션을 지원해야 함|
|-e|`Telnet Client Prompt`에 입력할 이스케이프 문자|
|-f|Client Logging에 사용할 파일 명|
|-t|터미널 형식 지정, <br>`vt100`, `vt52`, `ansi`, `vtnt`|
|host|원격 시스템 `host` 명 혹은 `IP`주소 지정|
|port|Port 번호 혹은 서비스 이름 지정|



**간단하게 아래 명령어** 처럼 **특정 Host나 주소에 특정 Port에 대해 테스트를 수행**할 수 있다.


```cmd
// 10.10.10.10:3333 으로 연결 시도
telnet 10.10.10.10 3333
```