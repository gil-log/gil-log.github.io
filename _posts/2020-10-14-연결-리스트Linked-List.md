---
title: "연결 리스트(Linked List)"
last_modified_at: 2020-10-14T03:21
categories: 
  - 자료구조
tags: 
  - 'linked list' 
  - '자료구조'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---



# 1. Linked List 란?



> 연결 리스트(Linked List)는 **각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식**으로 데이터를 저장하는 자료 구조이다. 이름에서 말하듯이 데이터를 담고 있는 노드들이 연결되어 있는데, **노드의 포인터가 다음이나 이전의 노드와의 연결을 담당**하게 된다.


![](https://images.velog.io/images/gillog/post/bc01facc-fb7b-4f4c-b12f-f8a23ff4b6e1/1.png)

연결 리스트는 그림처럼 Data와 다음 노드를 가르키는 포인터가 연이어 연결된 방식이다.


**연결 리스트는 Array와 달리 정해진 크기가 없고 동적으로 크기**를 늘릴 수 있는 장점이 있다. 

Linked List는 Array와 달리 물리적 저장 순서와 논리적 저장 순서가 일치하지 않기 때문에 원하는 위치에 삽입을 하고자 하면 원하는 위치를 검색하는 과정에 있어서 첫번째 Node부터 마지막 Node까지 일일이 확인 해봐야 한다. 
**검색에도 O(n)의 시간 복잡도** 를 가지고, **삽입, 삭제에 대해서는 O(1)의 시간복잡도**를 갖지만 **노드의 중간지점까지 검색을 통해 찾아가므로 O(n)의 시간 복잡도**를 갖는다.
(**맨 앞이나 맨 뒤에 원소를 삽입하거나 삭제 한다면 O(1)의 시간 복잡도**를 갖는다.)


Linked List를 Array와 비교해보면 아래와 같다.

<br>

**Array(배열)**
- 논리적 저장순서와 물리적 저장 순서가 일치.
- **인덱스로 시간 복잡도 O(1)만에 해당 원소에 접근**.
(즉, Random Access 가능)
- **제한적인 크기**를 갖는다.
- **삭제 또는 삽입 연산**시에 해당 원소에 접근하여 작업을 완료한 뒤 **Shift를 해주기 때문에 O(n)의 시간복잡도**를 갖는다.
(삭제 : 삭제 원소보다 인덱스가 큰 원소들 `Shift`, 삽입 : 다른 모든 원소 인덱스 1씩 `Shfit`)

- 즉, **검색이 잦은 경우 Array를 사용**하는 것이 좋다.




<br>




**Linked List(연결 리스트)**

- 논리적 저장순서와 물리적 저장 순서가 불일치.
(자료의 주소 값으로 노드를 이용해 서로 연결되어 있는 구조)
- 데이터를 추가 할때마다 **동적으로 크기**가 늘어난다.
- 원소 **검색 시** 첫번째 Node부터 마지막 Node까지 **일일이 확인하기 때문에 O(n)의 시간 복잡도**를 갖는다.
- **삽입 또는 삭제 연산**시에 해당 **원소를 검색한 후 삭제, 삽입 연산이 이루어지므로 O(n)의 시간 복잡도**를 갖는다.
(**맨 앞이나 맨 뒤에 원소를 삭제 하거나 삽입 한다면 O(1)의 시간 복잡도**를 갖는다.)

- 즉, **삽입, 삭제가 잦은 경우 Linked List**를 사용하는 것이 좋다.




Linked List 는 Tree 구조의 근간이 되는 자료구조이며, Tree 에서 사용되었을 때 그 유용성이 드러난다.


연결 리스트의 종류로는 단일 연결 리스트, 이중 연결 리스트 등이 있다.

![](https://images.velog.io/images/gillog/post/0ff09344-8911-4ec3-9df4-81e45bdf6e45/2.png)

**단일 연결 리스트(Singly Linked List)**는 앞서 살펴 본 그림 처럼 **다음 노드를 가리키는 포인터만을 가지는 연결 리스트**이다.


![](https://images.velog.io/images/gillog/post/5dc9360c-adbf-4bd3-9b9c-3a0e93176ed4/3.png)



**이중 연결 리스트(Doubly Linked List)**는 **다음 노드와 이전 노드를 가리키는 포인터**를 가지는 연결 리스트로, **단일 연결 리스트가 현재 요소에서 이전 요소로 접근하기가 어려운 단점을 극복**한다.


<br>

# 2. Linked List 사용법

Linked List를 사용하려면 java util패키에 LinkedList클래스를 import 해준다.

```import java.util.LinkedList;```

그 후 제너릭 타입으로 아래와 같이 생성해준다.

```java
//제너릭 타입 객체만 사용 가능
LinkedList<E> linkedList = new LinkedList<E>();

//Object 타입으로 선언됨
LinkedList linkedList = new LinkedList();
```


## 2.1 삽입

```java

LinkedList<Integer> linkedList = new LinkedList<Integer>();

// 가장 앞에 데이터 추가
linkedList.addFirst(data);

// 가장 뒤에 데이터 추가
linkedList.addLast(data);

// 가장 뒤에 데이터 추가
linkedList.add(data);

// index 뒤에 데이터 추가

linkedList.add(index,data);


// index의 데이터를 변경

linkedList.set(index, changeData);

```


## 2.2 삭제

```java

//Array.asList 사용 Linked List 생성
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));

//가장 앞의 데이터 제거
linkedList.removeFirst(); 

//가장 뒤의 데이터 제거
linkedList.removeLast(); 

//index 생략시 0번째 index제거
linkedList.remove(); 

//index의 data 제거
linkedList.remove(index);

//List안의 모든 data 제거
linkedList.clear();

```

## 2.3 검색

```java

//Array.asList 사용 Linked List 생성
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));

//list에 data가 있는지 검색 있다면 true를 return
linkedList.contains(data); 

//list에 data가 있는 index 반환, 없으면 -1을 return
linkedList.indexOf(data);

```

## 2.4 출력

```java

//Array.asList 사용 Linked List 생성
LinkedList<Integer> linkedList = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));

int a = 0 ;

// index에 있는 data 출력
a = linkedList.get(index);

// 첫번째 data 출력
a = linkedList.getFirst();

// 마지막 data 출력
a = linkedList.getLast();

// Linkde List size 크기 출력
size = linkdeList.size();


// 처음부터 끝의 data를 Array로 변환
// 타입은 제너릭으로 해야한다.
Integer array[] = linkedList.toArray(new Integer[linkedList.size()]);

```

## 2.5 그 외 메소드

|타입|메소드|설명|
|:---:|:---:|:---:|
|boolean|add(E e)|e를 리스트의 맨 끝에 추가|
|void|add(int index, E e)|index 위치에 e를 리스트에 추가|
|boolean|addAll(Collection<? extends E> c)|Collection인 c 전체를 리스트 맨 끝에 추가|
|boolean|addAll(int index, Collection<? extends E> c)|index 위치에 c 전체를 리스트에 추가|
|void|addFirst(E e)|리스트의 시작부분에 e를 추가|
|void|addLast(E e)|리스트의 끝부분에 e를 추가|
|void|clear()|리스트의 내용을 전부 삭제|
|boolean|contains(Object o)|리스트에 o가 있다면 true, 없으면 false|
|Iterator<E>|descendingIterator()|역방향으로 순환하는 iterator를 반환|
|E|get(int index)|index 위치의 값을 반환|
|E|getFirst()|리스트의 첫 요소를 반환|
|E|getLast()|리스트의 마지막 요소를 반환|
|int|indexOf(Object o)|o가 있는 인덱스를 반환, 없으면 -1 반환|
|E|remove()|리스트의 첫 요소를 반환 후 제거|
|E|remove(int index)|리스트의 index 위치의 요소를 반환 후 제거|
|E|removeFisrt()|리스트의 첫 요소를 제거 후 반환|
|E|removeLast()|리스트의 마지막 요소를 제거 후 반환|
|E|set(int index, E element)|index 위치의 값을 element로 변경|
|int|size()|현 리스트의 크기를 반환|
|Object[]|toArray()|현재 리스트를 배열로 변환 후 반환|



출처: https://eskeptor.tistory.com/89 [Hello World]





# 🙆‍♂️ 참고사이트 🙇‍♂️

[tech-interview-for-developer[gyoogle님]](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Data%20Structure/Linked%20List.md)

[Interview_Question_for_Beginner[JaeYeopHan님]](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure)

[[자료구조] Array vs LinkedList[VictoryWoo님]](https://woovictory.github.io/2018/12/27/DataStructure-Diff-of-Array-LinkedList/)

[[Java] 자바 LinkedList 사용법 & 예제 총정리[코딩팩토리]](https://coding-factory.tistory.com/552)

[[Array관련 두번째 이야기(LinkedList)][eskeptor님]](https://eskeptor.tistory.com/89https://eskeptor.tistory.com/89)