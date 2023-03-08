---
title: "[Protocol] STOMP"
last_modified_at: 2020-11-02T23:17
categories: 
  - 개념
tags: 
  - 'protocol' 
  - 'stomp'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# STOMP(Simple/Stream Text Oriented Message Protocol)

`STOMP`는 Simple/Stream Text Oriented Messaging Protocol 약자다. 

말 그대로 **간단한 문자 기반 메세징 프로토콜**이다. 

`프로토콜`이란 **원거리에서 메세지를 서로 주고 받을때 정의된 양식 규칙 체계**이다. 

다시말해 `STOMP`는 **웹 상에서 텍스트 송,수신을 위해 미리 정의된 특정한 규칙**이다.

STOMP에 정의한 규칙만 잘 지키면 여러 언어, 여러 플랫폼간에서 메세지를 상호 운영할 수 있다.


`STOMP`는 기존 AMQP 나 MQTT 와 같은 **메세지 전송을 위한 다른 프로토콜들과 다르게 binary 기반이 아닌 텍스트 기반 프로토콜**이다.

그렇기 때문에 **개발자가 읽기 쉽고 사용하기에 좋다. **




# 전송 방식


`STOMP`는 **HTTP와 마찬가지로 프레임(frame)을 사용해 전송하는 프로토콜**이다.
_프레임이란 주소와 명령, 명령 수행을 위한 데이터가 모두 포함된 데이터를 의미한다._

기본적으로는** 텍스트 기반 통신을 사용하지만, 바이너리 기반 통신도 지원**한다.

메시지 전송 방식은 **`TCP` 위에서 STOMP에서 정의한 Frame 구조로 Client와 Server 상호간에 메세지를 전달**한다.

**`STOMP`는** **메시지를 수신 할 대상 집합을 관리하는 일**을 한다.

**`STOMP`는 메시지에 대한 스팩만을 정의**하고 있기 때문에, **기능 구현은 전적으로 서버에 맡긴다.**





### Frame 구조

**`Frame`**은 **`명령(Command)`과 추가적인 `헤더(Header)`와 추가적인 `바디(Body)`로 구성**이 된다. 



`Frame`은 **몇 개의 텍스트 라인으로 지정된 구조**인데 **첫번째 라인은 텍스트(Command 명령어)**이고 **이후 key:value 형태로 header 정보를 포함**한다. 

**`header`이후에  공백 줄을 하나 더 추가하는 것**으로 **`header`의 끝을 설정**할 수 있다.

`header`이후에는 **Payload(Body)**가 존재한다. 
_페이로드(Payload)는 전송되는 데이터를 의미. <BR>[EX] JSON에서 DATA 부분. <BR>[EX] 택배 배송을 보내고 받을 때, 택배 물건이 payload._

**  Payload(데이터)는 Body에 위치하는데, 끝은 NULL 문자로 설정**한다.
  
아래 Frame의 구조를 보면 HTTP 요청과 왜 유사한지 알 수 있다.

```
COMMAND 
header1:value1 
header2:value2 

Body^@

```

### COMMAND

`COMMAND`에 사용 가능한 명령어들은 아래와 같고 상세 구현 내용은 [STOMP 공식 페이지](https://www.joinc.co.kr/w/man/12/STOMP)에서 확인 할 수 있다.

- CONNECT
- SEND
- SUBSCRIBE
- UNSUBSCRIBE
- BEGIN
- COMMIT
- ABORT
- ACK
- NACK
- DISCONNECT




<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[STOMP(Simple/Stream Text Oriented Message Protocol)[무명소졸의 웹개발]](https://warpgate3.tistory.com/183)

[STOMP Protocol Specification, Version 1.2[STOMP 공식 reference]](https://stomp.github.io/stomp-specification-1.2.html#STOMP_Frames)

[STOMP[joinc.co.kr]](https://www.joinc.co.kr/w/man/12/STOMP)

[]()

[]()

[]()