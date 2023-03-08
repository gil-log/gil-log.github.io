---
title: "[Java] Java8 설계의 밑바탕 - Stream 처리"
last_modified_at: 2021-06-09T00:55
categories: 
  - modernjavainaction
tags: 
  - 'Java' 
  - 'Java 8' 
  - 'Modern Java In Action' 
  - 'stream'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[Modern Java in Action](http://www.kyobobook.co.kr/product/detailViewEng.laf?ejkGb=BNT&mallGb=ENG&barcode=9781617293566)


---


# Stream 처리

**`Java 8` 의 설계의 밑바탕**이 되는 **세 가지 프로그래밍 개념**이 있다.

**그 중 하나가`Stream 처리`**이다.

**`Stream`이란 한 번에 한 개씩 만들어지는 연속적인 Data 항목들의 집합**이다.


`Java 8`의 Stream 관련 내용을 살펴보기 전 **Stream 처리에 대해서 먼저 예시**로 살펴보려 한다.


이론상 **Program은 입력 Stream에서 Data를 한 개씩 읽어 들이고**, **출력 Stream에서 Data를 한 개씩 기록**한다.

예시로 **Unix나 Linux의 많은 Program은 표준 입력(Unix, C의 `stdin`, Java의 `System.in`)에서 Data를 읽은 후**에,

**Data를 처리 결과를 표준 출력(Unix, C의 `stdout`, Java `System.out`)으로 기록**한다.

Unix의 **`cat` 명령어는 두 파일을 연결해서 Stream을 생성하고, `tr`은 Stream 문자열을 번역하고, `sort`는 Stream의 행을 정렬하고, `tail -3`은 Stream의 마지막 3개 행을 출력**한다.


아래 명령어를 살펴보자,

`cat gillog1 gillog2 | tr "[A-Z]" "[a-z]" | sort | tail -3`

이 명령어는 **`gillog1`, `gillog2` 파일의 단어를 소문자로 바꾼 후 사전 순으로 단어를 정렬 할때 가장 마지막 세 단어를 출력해주는 명령어**이다.

**Unix에서 파이프라인(|)으로 연결한 여러 명령어들(`cat`, `tr`, `sort`, `tail`)은 병렬로 실행**한다.

그래서 위 명령어 예제에서 **앞선 `cat`이나 `tr` 명령이 완료되지 않은 시점에서도 `sort`가 행을 처리하기 시작**할 수 있다.


---

# java.util.stream

**`Java 8`에서는 `java.util.stream` 패키지에 Stream API가 추가**되었다.

**Stream 패키지에 정의된 `Stream<T>`는 `T` 형식으로 구성된 일련의 항목을 의미**한다.

<br>

이전 **Unix 명령어 예제에서 복잡하게 파이프라인을 통해 명령어를 조합하여 Stream을 살펴본 것처럼**,

**Stream API는 파이프라인을 만드는데 필요한 많은 Method 들을 제공**한다.

## Stream API 핵심

**Stream API의 핵심**은 **기존에 Java에서는 한 번에 한 항목을 처리**했지만,

이제 **작업을 고수준으로 추상화하고 일련의 Stream으로 만들어 처리할 수 있다는 것**이다.

또한 **Stream 파이프라인을 이용하면 입력 부분을 여러 CPU Core에 할당**할 수 있다.

**Thread라는 복잡한 작업을 사용하지 않으면서도 병렬성을 손쉽게 챙길 수 있는 것**이다.


