---
title: "Java 성능 시간 측정"
last_modified_at: 2021-05-03T00:07
categories: 
  - 자바성능튜닝이야기
tags: 
  - 'JMH' 
  - 'Java' 
  - 'SYSTEM' 
  - 'currentTimeMillis' 
  - 'nanoTime' 
  - '자바 성능 튜닝 이야기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>


작성한 **자신의 메소드의 성능을 측정하는 방법에는 여러가지가 존재**한다.

이중 **System Class에서 제공하는 static 시간 측정 메소드로 대표적인 `currentTimeMilis()` 와 `nanoTime()` 메소드**가 있다.

# static long currentTimeMillis()

**`currentTimeMillis()`는 현재 시간을 ms로 return**한다.

**`UTC`라는 시간 표준 체계를 따르며, 1970년 1월 1일 부터 시간을 long 타입으로 return**한다.
_대한민국은 UTC+9 지역이다._

따라서 **호출 할 때마다 값이 다르다.**

**이 시간 값을 변환하여 현재 날짜를 구할 수도** 있다.


# static long nanoTime()

**`nanoTime()`는 JDK 5.0부터 추가된 메소드**이다.

**현재 시간을 ns로 return**한다.

**1ms 이하의 nano 수행 시간 측정을 위해 만들어진 메소드**로 **오늘 날짜를 알아내는 부분에서는 사용하지 않고, 성능 측정 용도로 사용하는 메소드**이다.


---

# JMH

**`JMH`는 Java Microbenchmark Harness의 약자**로, **JDK를 오픈 소스로 제공하는 OpenJDK에서 만든 Java 성능 측정용 라이브러리**다.

이 외에도

**`Caliper`, `JUnitPerf`, `JUnitBench`, `ContiPerf`등의 측정 라이브러리 들이 존재**한다.

**`JMH`와 `Caliper`를 제외한 나머지 Tool들은 `JUnit` 테스트 코드 실행에 사용**된다.

<br>
아래 예제는 `JMH`로 HashMap과 ArrayList 객체 생성 속도를 비교해보는 예제이다.

```java
package com.perf.timer;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.concurrent.TimeUnit;

import org.openjdk.jmh.annotations.BenchmarkMode;
import org.openjdk.jmh.annotations.GenerateMicroBenchmark;
import org.openjdk.jmh.annotations.Mode;
import org.openjdk.jmh.annotations.OutputTimeUnit;

// JMH Class 선언 시 반드시 Annotation을 지정할 필요는 없지만, 이렇게 하면 기본 옵션으로 수행되기에 평균 응답 시간 측정을 위해 @BenchmarkMode로 옵션을 지정
@BenchmarkMode({ Mode.AverageTime })
// @OutputTimeUnit Annotation을 사용하여 출력 시간 단위를 밀리초로 지정, 기본 옵션이 밀리이지만 시간 단위를 옵션으로 명시할 수 있는 예시
@OutputTimeUnit(TimeUnit.MILLISECONDS)
public class CompareTimerJMH {

    // 측정 대상이 되는 메소드를 선언할 때 사용, 해당 클래스에 메소드가 많이 있어도 이 Annotation을 설정하지 않으면 테스트 대상 제외
    @GenerateMicroBenchmark
    public DummyData makeObject(){
        HashMap<String, String> map = new HashMap<String, String>(1000000);
        ArrayList<String> list = new ArrayList<String>(1000000);
        return new DummyData(map, list);
    }
}
```

<br>

아래는 JMH 테스트 실행 결과이다.

```
$ java -jar target/microbenchmarks.jar .*CompareTimerJMH.* -i 3 -r 1s
# Measurement Section
# Runtime (Per iteration): 1s
# Iterations: 3
# Thread counts (concurrent threads per iteration): [1, 1, 1]
# Threads will synchronize iterations
# Benchmark mode: Average time, time/op
# Running: com.perf.timer.CompareTimerJMH.makeObjectWithSize1000000
# Warmup Iteration	1 (3s in 1 thread): 2.691 msec/op
# Warmup Iteration	2 (3s in 1 thread): 2.131 msec/op
# Warmup Iteration	3 (3s in 1 thread): 2.317 msec/op
# Warmup Iteration	4 (3s in 1 thread): 2.172 msec/op
# Warmup Iteration	5 (3s in 1 thread): 2.134 msec/op
Iteartion	1 (1s in 1 thread): 2.011 msec/op
Iteartion	2 (1s in 1 thread): 2.215 msec/op
Iteartion	3 (1s in 1 thread): 2.311 msec/op

Run result "makeObjectWithSize1000000": 2.089 ±(95%) 0.160 ±(99%) 0.369 msec/op
Run statics "makeObject": min = 2.011, avg = 2.114, max = 2.311, stdev = 0.057
Run confidence intervals "makeObjectWithSize1000000": 95% [1.929, 2.249], 99% [1.720, 2.458]

Benchmark			Mode	Thr	Cnt	Sec	Mean	Mean	error	Units
c.p.t.CompareTimerJMH.makeObject avgt	1	3	1	2.098	0.369 msce/op
```

위 처럼 **`JMH`는 여러개 `스레드 단위 테스트`도 가능**하고, **`워밍업 작업`도 자동 수행하기에 정확한 성능 측정이 가능**하다.

