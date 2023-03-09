---
title: "[Java] Set 구현 Class 중 가장 빠른 Class는??"
last_modified_at: 2021-05-18T00:36
categories: 
  - java-performance-tuning-story
tags: 
  - 'Java' 
  - 'set' 
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

# Fastset Class in Set implements Class

`Set` Interface를 구현한 Class 중 가장 빠른 Class를 직접 테스트해보기 위해 `jmh` 테스트 코드를 작성해보았다.


## Data 저장

```java
package com.tuning.jmh;

import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;
import java.util.TreeSet;
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
public class FastestClassInSet {
    int LOOP_COUNT = 1000;
    Set<String> set;
    String data = "gillogfastestsetimplementaions";

    @GenerateMicroBenchmark
    public void addHashSet() {
        set = new HashSet<String>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }
    
    @GenerateMicroBenchmark
    public void addTreeSet() {
        set = new TreeSet<String>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }

    @GenerateMicroBenchmark
    public void addLinkedHashSet() {
        set = new LinkedHashSet<String>();
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }
}
```

위 테스트 코드는 **`Set` Interface를 구현한 `HashSet`, `TreeSet`, `LinkedHashSet`의 Data 저장 과정의 성능 차이를 비교해보기 위한 코드**이다.

위 테스트의 결과는 아래와 같다.

|Target|Response Sec(micro)|
|:--:|:--:|
|HashSet|375|
|TreeSet|1,214|
|LinkedHashSet|378|

**`HashSet`과 `LinkedHashSet`의 성능은 비슷한 것으로 파악**되었고, **`TreeSet`의 순서 정렬 수행 작업으로 인해서 가장 성능이 뒤쳐지는 것으로 결과**가 나왔다.


<br>

### Data Size를 알고 있다면??

현재 Data의 크기를 알고 있으므로 **`HashSet`에 크기를 지정해서 테스트**를 해보면 아래와 같다.

```java
    @GenerateMicroBenchmark
    public void addHashSetWithInitialSize() {
        set = new HashSet<String>(LOOP_COUNT);
        for(int loop = 0; loop < LOOP_COUNT; loop++) {
            set.add(data+loop);
        }
    }
```

|Target|Response Sec(micro)|
|:--:|:--:|
|HashSet|375|
|HashSetWithIntialSize|353|

큰 차이는 발생하지 않았지만, **Data 크기를 미리 알고 있을 경우 객체 생성시 크기를 미리 지정하는 것이 성능상 유리**하다.



## Data 출력

이번에는 **`Set`을 구현한 Class들이 Data를 출력할 때 얼마나 많은 차이가 발생하는지 확인**해보려 한다.

테스트 코드는 아래와 같다.

```java
package com.tuning.jmh;

import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;
import java.util.TreeSet;
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
public class FastestClassInSet {
    int LOOP_COUNT = 1000;
    Set<String> hashSet;
    Set<String> treeSet;
    Set<String> linkedHashSet;
    String data = "gillogfastestsetimplementaions";
    String []keys;
    String result = null;
    
    @Setup(Level.Trial)
    public void setUp() {
        hashSet = new HashSet<String>();
        treeSet = new TreeSet<String>();
        linkedHashSet = new LinkedHashSet<String>();
        for(int loop=0;loop<LOOP_COUNT;loop++) {
            String tempData = data + loop;
            hashSet.add(tempData);
            treeSet.add(tempData);
            linkedHashSet.add(tempData);
        }
    }

    @GenerateMicroBenchmark
    public void iterateHashSet() {
        Iterator<String> iterator = hashSet.iterator();
        while(iterator.hasNext()) {
            result = iterator.next();
        }
    }
    
    @GenerateMicroBenchmark
    public void iterateTreeSet() {
        Iterator<String> iterator = treeSet.iterator();
        while(iterator.hasNext()) {
            result = iterator.next();
        }
    }

    @GenerateMicroBenchmark
    public void iterateLinkedHashSet() {
        Iterator<String> iterator = linkedHashSet.iterator();
        while(iterator.hasNext()) {
            result = iterator.next();
        }
    }
}
```

테스트 결과는 아래와 같다.

|Target|Response Sec(micro)|
|:--:|:--:|
|HashSet|26|
|TreeSet|35|
|LinkedHashSet|16|

**`LinkedHashSet` Class가 가장 빠르게 Data를 읽고**, **`HashSet`, `TreeSet` 순으로 성능 차이**가 나는걸 알 수 있다.

**일반적으로 `Set`은 여러 Data를 저장하고 해당 Data가 존재하는지를 확인하는 용도**로 가장 많이 사용된다.

**따라서 `Iterator`로 Data를 읽어오는것이 아니라 Random 하게 Data를 가져와야 한다.**

**이번에는 Random하게 Data를 읽어오는 부분에서 성능 테스트를 진행**해보려 한다.

Data를 Random하게 읽어오기 위해 아래와 같은 Class를 생성하였다.

```java
package com.tuning.jmh;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.Random;
import java.util.Set;

public class RandomKeyUtil {

    public static String[] generateRandomSetKeysSwap(Set<String> set) {
        int size = set.size();
        String[] result = new String[size];
        Random random = new Random();
        int maxNumber = size;
        Iterator<String> iterator = set.iterator();
        int resultPos = 0;
        while (iterator.hasNext()) {
            result[resultPos++] = iterator.next();
        }
        for(int loop=0;loop<size;loop++){
            int randomNumberOne = random.nextInt(maxNumber);
            int randomNumberTwo = random.nextInt(maxNumber);
            String temp = result[randomNumberTwo];
            result[randomNumberTwo] = result[randomNumberOne];
            result[randomNumberOne] = temp;
        }
        return result;
    }
}
```

**`generateRandomSetKeysSwap` method를 통해 Data의 개수 만큼 불규칙적인 key**를 가져올 수 있다.

이제 **비순차적으로 Data를 읽어와 성능 테스트를 수행하는 테스트 코드는 아래**와 같다.

```java
package com.tuning.jmh;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.Random;
import java.util.Set;
import java.util.concurrent.TimeUnit;

import org.open.jdk.jmh.annotations.BenchmarkMode;
import org.open.jdk.jmh.annotations.GenerateMicroBenchmark;
import org.open.jdk.jmh.annotations.Mode;
import org.open.jdk.jmh.annotations.OutputTimeUnit;
import org.open.jdk.jmh.annotations.Scope;
import org.open.jdk.jmh.annotations.State;
import com.tuning.jmh.RandomKeyUtil;

@State(Scope.Thread)
@BenchmarkMode({ Mode.AverageTime })
@OutputTimeUnit(TimeUnit.MICROSECONDS)
public class SetContains {
    int LOOP_COUNT = 1000;
    Set<String> hashSet;
    Set<String> treeSet;
    Set<String> linkedHashSet;
    String data = "gillogfastestsetimplementaions";
    String []keys;
    
    @Setup(Level.Trial)
    public void setUp() {
        hashSet = new HashSet<String>();
        treeSet = new TreeSet<String>();
        linkedHashSet = new LinkedHashSet<String>();
        for(int loop=0;loop<LOOP_COUNT;loop++) {
            String tempData = data + loop;
            hashSet.add(tempData);
            treeSet.add(tempData);
            linkedHashSet.add(tempData);
        }
        if(keys==null || keys.length != LOOP_COUNT) {
            keys = generateRandomSetKeysSwap(hashSet);
        }
    }

    @GenerateMicroBenchmark
    public void containsHashSet() {
        for(String key : keys) {
            hashSet.contains(key);
        }
    }

    @GenerateMicroBenchmark
    public void containsTreeSet() {
        for(String key : keys) {
            treeSet.contains(key);
        }
    }

    @GenerateMicroBenchmark
    public void containsLinkedHashSet() {
        for(String key : keys) {
            linkedHashSet.contains(key);
        }
    }
}
```

해당 테스트의 결과는 아래와 같다.


|Target|Response Sec(micro)|
|:--:|:--:|
|HashSet|32|
|TreeSet|841|
|LinkedHashSet|32|

**`HashSet`과 `LinkedHashSet`의 속도**는 이전 **순차적인 Data Reading 테스트 결과와 비슷하게 측정**되었지만, **`TreeSet`의 속도는 현저히 느려진걸 확인**할 수 있다.

이는 **`TreeSet` Class는 Data를 저장하면서 정렬을 수행**한다.

`TreeSet`의 선언문을 보면 아래와 같은데,

```java
public class TreeSet<E> extends AbstractSet<E>
	implements NavigableSet<E>, Cloneable, Serializable
```

**구현 Interface 중 `NavigableSet` Interface**가 있다.

**`NavigableSet` Interface는 특정 값 보다 큰 값이나 작은 값, 가장 큰 값, 가장 작은 값 등을 추출하는 method를 선언**해 놓았고, **`JDK 1.6` 부터 추가된 Interface**이다.


이로인해 **`TreeSet`은 Data를 순서에 따라 탐색하는 작업이 필요한 경우 사용하는 것이 좋다는걸 확인**할 수 있다.

하지만 **순서에 따른 탐색 작업이 필요 없을 경우 `HashSet`이나 `LinkedHashSet`을 사용하는 것이 성능상 더 빠르다.**
