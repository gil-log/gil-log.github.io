---
title: "[Java] Java 8 전, 후의 Java History "
last_modified_at: 2021-06-07T00:35
categories: 
  - modernjavainaction
tags: 
  - 'Modern Java In Action'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[Modern Java in Action](http://www.kyobobook.co.kr/product/detailViewEng.laf?ejkGb=BNT&mallGb=ENG&barcode=9781617293566)


---

**`JDK 1.0(1996)`부터 `JDK 7(2011)` 까지 Java는 새로운 기능과 더불어 계속 발전을 거듭**했다.

**18년도 3월에는 `Java 10`, 18년도 9월에는 `Java 11`이 `Release`** 되었다.

Java 역사에서 각 Version 별로 무슨일이 일어났을까?

---

# Java History

**Java 역사를 통틀어 가장 큰 변화는 `Java 8`**에서 일어났다.

**Java 9에서도 중요한 변화**가 있었지만, **Java 8 만큼 획기적이거나 생산성이 바뀌지는 않았다.**

**Java 10에서는 형 추론과 관련한 약간의 변화**만 일어났다.

아래는 **gillog post 글을 조회수 순으로 정렬하는 고전 Java 코드** 이다.

```java
Collections.sort(gillog, new Comparator<Post>() {
	public int compare(Post p1, Post p2) {
    	return p1.getHits().compareTo(p2.getHits());
    }
});
```

**`Java 8`을 이용하면 더 간단하게 위 코드를 구현**할 수 있다.

```java
gillog.sort(comparing(Post::getHits));
```

<br>

**Multi Core CPU 대중화 같은 하드웨어적인 변화도 Java 8에 영향**을 끼쳤다.

**지금까지 대부분의 Java Program은 CPU Core 중 하나 만을 사용**했다.
_나머지 코어는 유효 idle 상태로, 운영체제, 백신 검사 프로그램과 Process Power를 공유하며 사용_


**Java 8 이전에는 나머지 CPU Core를 활용 하려면 Thread를 사용하는 방법으로 활용**하였다.


하지만 **Thread를 사용하면 관리하기 어렵고 많은 문제가 발생**할 수 있다.

**Java는 이러한 병렬 실행 환경을 쉽게 관리하고 Error가 덜 발생하는 방향으로 발전하며 노력**해왔다.

**`Java 1`에서는 `Thread`, `lock`, `Memory Model`까지 지원**했다.

하지만 특별 전문가들이 아니고서야 **이와 같은 저수준 기능을 온전히 활용하기 어려웠다.**

<br>

**`Java 5`**에서는 **`Thread Pool`, `concurrent collection(병렬 실행 컬렉션)` 등 아주 강력한 도구를 도입**했다.

**`Java 7`에서는 병렬 실행에 도움을 줄 수 있는 포크/조인 프로엠워크를 제공**했지만, 
아직까지도 이를 **온전히 활용하기란 쉽지 않았다.**

**`Java 8`에서는 병렬 실행을 새롭고 단순하게 접근할 수 있는 방법을 제공**한다.

**`Java 9`에서는 `리액티브 프로그래밍`이라는 병렬 실행 기법을 지원**한다.

**위 기법은 사용 가능 상황이 상당히 한정적**이지만,
**수요가 많은 고성능 병렬 시스템에서 인기**를 얻고 있는 **`RxJava(Reactive Stream ToolKit)`을 표준적인 방식으로 지원**한다.

<br>

**`Java 8`은 간결한 코드, MulitCore Process의 쉬운 활용이라는 두 가지 요구사항을 기반**으로 한다.

**`Java 8`에서 제공하는 새로운 기술은 간단하게 표현**하면 아래와 같다.

- Stream API
- Method에 Code를 전달하는 기법
- Interface의 Default Method

`Java 8`은 **Database 질의 언어(SQL)에서 표현식을 처리하는 것 처럼**,
**병렬 연산을 지원하는 Stream 이라는 새로운 API를 제공**한다.

**SQL에서 고수준 언어로 원하는 동작을 표현**하면,

**구현(Java에서는 Stream Library가 이 역할을 수행)에서 최적의 저수준 실행 방법을 선택하는 방식으로 동작**한다.

**`Stream`을 이용**하면 **Error를 자주 일으키고, MultiCore CPU를 이용하는 것 보다 비용이 훨씬 비싼 Keyword `synchronized`를 사용하지 않아도 된다.**

_**MultiCore CPU의 각 Core는 별도의 Cache를 포함**하고 있는데, 
**lock을 사용하면 이러한 cache가 동기화** 되어야 하므로 **속도가 느린 캐시 일관성 프로토콜 인터코어 통신(cache-coherency-protocol intercore communication)**이 이루어진다._

조금 다른 관점에서 보자면 **결국 Java 8에 추가된 `Stream API` 덕분**에,

**다른 두 가지 기능인 Method에 Code를 전달하는 간결 기법(Method 참조, 람다)**와 **Interface의 default method가 존재** 할 수 있다.