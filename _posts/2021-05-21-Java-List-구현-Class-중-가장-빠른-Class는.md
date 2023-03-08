---
title: "[Java] List 구현 Class 중 가장 빠른 Class는??"
last_modified_at: 2021-05-21T00:31
categories: 
  - 자바성능튜닝이야기
tags: 
  - 'Java' 
  - 'List' 
  - '자바 성능 튜닝 이야기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>

---

# Fastset Class in List implements Class

**`List` Interface를 구현한 `ArrayList`, `LinkedList`, `Vector` Class 들의 성능 차이를 비교**해보려한다.



## Data 저장


아래 `JMH`로 작성한 테스트 코드는 **세 가지 Calss의 Data 추가 성능 테스트 코드**이다.

```java
package com.tuning.jmh.story04.list;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Vector;
import java.util.concurrent.TimeUnit;

import org.open.jdk.jmh.annotations.BenchmarkMode;
import org.open.jdk.jmh.annotations.GenerateMicroBenchmark;
import org.open.jdk.jmh.annotations.Mode;
import org.open.jdk.jmh.annotations.OutputTimeUnit;
import org.open.jdk.jmh.annotations.Scope;
import org.open.jdk.jmh.annotations.State;

@State(Scope.Thread)
@BenchmarkMode({ Mode.AverageTime })
@OutputTimeUnit(TimeUnit.MICROSECONDS)
public class FastestClassInListInAdd {
    int LOOP_COUNT = 1000;
    List<Integer> arrayList;
    List<Integer> vector;
    List<Integer> linkedList;

    @GenerateMicroBenchmark
    public void addArrayList() {
        arrayList = new ArrayList<Integer>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            arrayList.add(loop);
        }
    }
    
    @GenerateMicroBenchmark
    public void addVector() {
        vector = new Vector<Integer>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            vector.add(loop);
        }
    }

    @GenerateMicroBenchmark
    public void addLinkedList() {
        linkedList = new LinkedList<Integer>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            linkedList.add(loop);
        }
    }
}
```

위 테스트 코드의 결과는 아래 표와 같다.

|Target|Response Sec(micro)|
|:--:|:--:|
|ArrayList|28|
|Vector|31|
|LinkedList|40|

위 성능 표와 같이 **`ArrayList`, `Vector`, `LinkedList` 순으로 빠른 성능**을 보여주었다.

하지만 **어느 Class던 Data를 저장하는 부분에서는 큰 성능 차이는 보이지 않았다.**


## Data 출력