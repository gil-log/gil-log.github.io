---
title: "Profiling Tool Feature - Respone Time Profiling, Memory Profiling"
last_modified_at: 2021-04-28T12:04
categories: 
  - java-performance-tuning-story
tags: 
  - 'Memory Profiling' 
  - 'Profilling Tool' 
  - 'ResponseTime Profiling' 
  - '메모리 프로파일링' 
  - '응답 시간 프로파일링' 
  - '자바 성능 개선 이야기' 
  - '프로파일링'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>

**`Profiling Tool`은 대부분 느린 메소드, 클래스 검색을 목적**으로 한다.
_**`APM Tool`은 목적이 상이**한데, ** 주로 문제점 진단이나 시스템 모니터링 운영**에 강하다._

**Java Application 분석 `Profiling Tool`은 크게 상용, 비 상용으로 나뉜다.**


### 상용 Profiling Tool

`상용 Tool`로는 **`Quest Software`의 `JProbe`**와 **`ej-technologies`의 `JProfiler`**, **`YourKit`의 `YourKit`이라는 Tool**이 있다.


### 비 상용 Profiling Tool

`비 상용 Tool`로는 **`넷빈즈`에서 사용하는 `Profiler`**가 있고, **`Eclipse`에서 사용하는 `TPTP(Eclipse Test & Performance Tools Platform)**도 있다.

_이 **TPTP Tool은 Range가 굉장히 넓어**서 **Profiling에 대한 내용 공부만 해도 양이 상당**하다._

---

<br>

**`Profiling Tool`이 제공하는 기본 기능 중 각각의 Tool들이 제공하는 기능은 다양하고 상이**하지만, **`응답 시간 프로파일링(Response Time Profiling)`과 `메모리 프로파일링(Memory Profiling)`기능은 기본적으로 제공**한다.

# Response Time Profiling

**`Response Time Profiling`을 시행하는 이유는 너무 당연하게도 응답 시간 측정을 위해서**다.

**하나의 클래스 내에 사용되는 메소드 단위 응답 시간을 측정**한다.

**Tool에 따라 소스 라인 단위 응답 속도 측정을 할 수도** 있다.

**`Response Time Profiling` 시에는** 보통 **`CPU 시간`, `대기 시간`** 이렇게 **두 가지 시간이 제공**된다.

## CPU 시간과 대기시간??

**하나의 메소드, 한 소스 라인을 수행하는데 소요되는 시간은 무조건 `CPU 시간`과 `대기 시간`으로 나뉜다.**

**`CPU 시간`은 CPU를 점유한 시간을 의미**하고, **`대기 시간`은 CPU를 점유하지 않고 대기하는 시간을 의미**한다.

따라서 **`실제 소요 시간(Clock Time)` = `CPU 시간` + `대기 시간` 으로 계산**할 수 있다.

**`CPU 시간`은 Tool에 따라 `Thread 시간`으로 표시되기도** 한다.

**`Thread 시간`**은 **해당 Thread에서 CPU를 점유한 시간이기 때문 표현만 다를 뿐 같은 시간**이다.

# Memory Profiling

**`Memory Profiling`을 시행하는 이유**는 **적은 횟수의 사용에도 불구**하고 **GC의 대상이 되는 부분을 찾거나**, **`Memory Leak`이 발생하는 부분을 찾기 위함**이다.

**`Memory Profiling`에는 클래스 및 메소드 단위의 메모리 사용량이 분석**된다.

**Tool에 따라 소스 라인 단위 메모리 사용량도 측정을 할 수도 있다.**



---

**`Profiling Tool`을 마법의 자동 분석 도구로 인지하는 경우가 많지만, `APM Tool`이던, `Profiling Tool`이던 자동으로 분석을 진행해주는 Tool은 없다.
**

**Tool에서 분석을 하려면 해당 메소드가 수행 되어야 하고, 수행 되지 않는 메소드는 분석이 되지 않는다.**


**문제 되는 메소드가 수행 되어야 하므로**, **메모리 부족 현상이 가장 분석하고 찾아내기 어렵다.**




