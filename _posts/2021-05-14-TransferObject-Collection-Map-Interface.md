---
title: "[TransferObject] Collection, Map Interface"
last_modified_at: 2021-05-14T00:05
categories: 
  - java-performance-tuning-story
tags: 
  - 'Collection' 
  - 'Java' 
  - 'Map' 
  - 'Spring' 
  - 'TransferObject' 
  - '자바 성능 튜닝 이야기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>


대부분 프로젝트에서 **`Transfer Object`를 많이 사용**하고, 그 내부에서 **`Collection`과 `Map` Interface를 상속받는 객체가 많이 사용**된다.

**목록 데이터를 담기에 가장 좋은것이 `Array`**이고, 그 다음으로 **`Collection` 관련 객체들이기 때문**이다.

**`Array`는 생성부터 크기를 지정**해야 하지만, **`Collection`객체 들은 그럴 필요 없이 자동으로 크기가 증가**된다.

이번엔 **어떤 객체를 써야 성능면에서 우수**한지 알아보려한다.

<br>

# Collection 및 Map Interface

**`Array`를 제외하면 Data를 담기 가장 좋은 객체**는 **`Collection`과 `Map` Interface를 상속한 객체**이다.

**`Collection`과 `Map`에 대해 간략히 정리**해보면 아래와 같다.

![](https://images.velog.io/images/gillog/post/e8c527d1-0b63-445d-a47b-9b829c327470/image.png)


|Interface|설명|
|:--:|:--:|
|Collection|가장 상위 Interface|
|Set|중복을 허용하지 않는 집합 처리용도 Interface|
|SortedSet|오름차순을 갖는 Set Interface|
|List|순서가 있는 집합 처리 용도 Interface, Index가 있어 위치를 지정해 값을 찾을 수 있다. <br> 중복을 허용, List Interface를 상속받는 Class 중 가장 많이 사용하는 것이 ArrayList|
|Queue|여러개 객체를 처리하기 전 담아 처리할 때 사용하는 Interface, FIFO|
|Map|Key와 Value 쌍으로 구성된 객체 집합을 처리하기 위한 Interface, 중복 키를 허용하지 않는다.|
|SortedMap|Key를 오름차순으로 정렬하는 Map Interface| 

---

## Set Interface

먼저 `Set` Interface에 대해 알아보면, **`Set` Interface는 중복이 없는 집합 객체를 만들때 유용하게 사용**된다.

예를들어 **전체 학생 이름이 몇개 인지 확인하기 위해서 중복되는 이름을 제외**해야 하지만, **`Set` 객체에 Data를 저장**하면 **중복되는 Data는 원천적으로 삽입되지 않는다.**

**`Set` Interface를 구현한 Class로는 `HashSet`, `TreeSet`, `LinkedHashSet` 세 가지**가 있다.


|Class|설명|
|:--:|:--:|
|HashSet|Data를 **Hash Table에 저장하는 Class로 순서 없이 저장**한다.|
|TreeSet|**Red-Black이라는 Tree 구조에 Data를 저장, 값에 따라 순서**가 정해진다.<br> Data를 담으면서 동시에 **정렬을 수행하여 `HashSet`보다 성능 상 느리다.**|
|LinkedHashSet|**Hash Table에 Data를 저장**하는데, **저장된 순서에 따라 순서가 결정**된다.|

### Red Black Tree??

**`Red-Black Tree`** 란 **이진 트리 구조로 Data를 담는 구조**를 말하며 **아래와 같은 특징**이 있다.

1. **각각의 노드는 검은색이나 붉은색** 이어야 한다.

2. **가장 상위(root) 노드는 검은색**이다.

3. **가장 말단(leaves) 노드는 검은색**이다.

4. **붉은 노드는 검은 하위 노드만**을 가진다.

5. **검은 노드는 붉은 상위 노드만**을 가진다.

6. **모든 말단 노드로 이동하는 경로의 검은 노드 수는 동일**하다.

![](https://images.velog.io/images/gillog/post/5e0daf53-8d83-4a43-a2f6-1dcf00f454e6/image.png)

---

## List Interface

**`List` Interface 는 `Array`의 확장판**이라고 보면 된다.

**`C`나, `Java`의 `Array`는 모두 최초 선언 시 담을 수 있는 Data의 개수를 한정하여 생성**한다.

하지만 **`List` Interface를 구현한 Class들은 담을 수 있는 크기가 자동으로 증가** 되므로, **Data의 개수를 확실히 모를 때 유용하게 사용**할 수 있다.

**`List` Interface를 구현한 Class**에는 **`ArrayList`, `Linked-List`, 원조 Class 격인 `Vector` Class**가 있다.

|Class|설명|
|:--:|:--:|
|Vector|**객체 생성 시 크기를 지정할 필요가 없는 Array Class**이다.|
|ArrayList|**Vector와 비슷**하지만, **동기화 처리가 되어 있지 않다.**|
|LinkedList|**ArrayList와 동일**하지만, **`Queue` Interface를 구현**했기 때문에, **FIFO Queue 작업을 수행**한다.|

---

## Map Interface

**`Map` Interface는 `Key`, `Value` 쌍으로 저장되는 구조체**이다.

**단일 객체만 저장되는 다른 `Collection`과는 다르게 따로 분리**되어 있다.

**`Map` Interface는** ID와 PassWord, Code와 이름 등 **고유한 값과 그 값을 설명하는 Data를 보관할 때 유용**하다.

**`Map` Interface를 구현한 Class**로는 , **`HashMap`, `TreeMap`, `LinkedHashMap` 세 가지가 있고, 원조 Class 격인 `Hashtable` Class**가 있다.


|Class|설명|
|:--:|:--:|
|Hashtable|**Data를 Hashtable에 담는 Class**로, **내부에서 관리하는 Hashtable 객체가 동기화**되어 있으므로, **동기화가 필요한 부분에서는 이 Class를 사용하는 것이 권장**된다.|
|HashMap|**Data를 Hashtable에 담는 Class**로, **`Hashtable` Class와 다른 점은 `null`값을 허용한다는 것과, 동기화 되어 있지 않다는 것**이다.|
|TreeMap|**`Red-Black Tree`에 Data**를 담는다. **`TreeSet`과 다른 점은 `Key`에 의해 순서가 정해진다는 것**이다.|
|LinkedHashMap|**`HashMap`과 거의 동일**하며, **`이중 연결 리스트(Doubly-LinkedList)`라는 방식을 사용하여 Data를 담는다는 점만 다르다.**|


### 이중 연결 리스트(Doubly-LinkedList) ???

**`이중 연결 리스트(Doubly-LinkedList)`** 는 자료구조론에서 **앞, 뒤 Node에 대한 Link 정보를 가지고 있는 것**을 말한다.

만약 **앞 Node의 Link 값이 `null`이거나 비어 있으면 가장 첫 Node임을 의미**하고,
**뒤 Node의 Link 값이 `null`이거나 비어 있으면** **가장 마지막 Node임을 의미**한다.

![](https://images.velog.io/images/gillog/post/ae7bfe8c-40b8-4a49-8f53-fd3b5179a122/image.png)



---

## Queue Interface

**`Queue`는 Data를 담아 두었다가 먼저 들어온 Data부터 처리하기 위해 사용**된다.

예를 들면 **SMS와 같은 문자를 처리**할 때 **서버에 들어오는 순서대로 `Queue`에 저장**하고 **처음 요청한 Data부터 처리**하면 된다.

<br>

이때 **`List`도 순서가 있고, `Queue`도 순서가 있는데, `Queue`를 사용하는 이유**는, `List`의 **가장 큰 단점은 첫 번째 Data(0번 Data)를 지우면 그 다음 두 번째 Data(1번 Data)**부터 **마지막 Data까지 한 칸씩 옮기는 작업을 수행**해야 하므로, **Data가 많으면 많을 수록 Data를 지우는데 소요되는 시간이 증가되기 때문**이다.

**`Queue` Interface를 구현한 Class는 두 가지**로 나뉘며, **`java.util` package에 속하는 `LinkedList`와 `PriorityQueue`는 일반적 목적의 `Queue` Class**이고, **`java.util.concurrent` package에 속하는 Class들은 `Concurrent Queue Class`**이다.

|Class|설명|
|:--:|:--:|
|PriorityQueue|**Queue에 추가된 순서와 상관없이 먼저 생성된 객체가 먼저 나오도록 되어 있는 Queue**|
|LinkedBlockingQueue|**저장할 Data의 크기를 선택적으로 정할 수 도 있는 FIFO기반 LinkNode를 사용하는 BlockingQueue**|
|ArrayBlockingQueue|**저장되는 Data의 크기가 정해져 있는 FIFO기반의 BlockingQueue**|
|PriorityBlockingQueue|**저장되는 Data의 크기가 정해져 있지 않고, 객체의 생성 순서에 따라서 순서가 저장**되는 BlockingQueue|
|DelayQueue|**Queue가 대기하는 시간을 지정하여 처리**하도록 되어 있는 Queue|
|SynchronousQueue|**put() method를 호출**하면, **다른 Thread에서 take() method가 호출될 때 까지 대기**하도록 되어있는 Queue, **해당 Queue에 저장되는 Data는 없다.**<br>**API에서 제공하는 대부분의 메소드는 0, null을 return**|
