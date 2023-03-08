---
title: "배열 리스트(Array List)"
last_modified_at: 2020-10-16T06:44
categories: 
  - 자료구조
tags: 
  - 'ArrayList' 
  - '자료구조'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 배열 리스트(Array List)란?

> 배열 리스트(Array List)는 List 인터페이스를 상속받은 클래스로 크기가 **가변적으로 변하는 선형리스트**다.
**일반적인 배열과 같은 순차리스트이며 인덱스로 내부의 객체를 관리한다는점이 유사**하지만 한번 생성되면 크기가 변하지 않는 배열과는 달리 **ArrayList는 객체들이 추가되어 저장 용량(capacity)을 초과한다면 자동으로 부족한 크기만큼 저장 용량(capacity)이 늘어난다는 특징**을 가지고 있다.

ArrayList는 List 인터페이스의 구현 클래스 이다. 일반 Array와 유사하지만 Array는 생성할 시 크기가 고정되지만, **ArrayList는 저장 용량 초과 객체가 들어오면 자동으로 저장 용량이 늘어난다.**

## ArrayList 특징



ArrayList에 객체를 추가하면 인덱스 0부터 차례로 저장된다.  
![](https://images.velog.io/images/gillog/post/728fd46b-e95d-4880-aa7e-3703a6be68c7/2.png)


ArrayList에서 특정 인덱스의 객체를 제거하게 되면 바로 뒤 인덱스 부터 마지막 인덱스까지 모두 1칸씩 인덱스가 당겨진다.

![](https://images.velog.io/images/gillog/post/1c2499df-8a8f-449d-86b9-ed0dc12984d0/1.png)

삽입 역시 특정 인덱스에 객체를 삽입하면 해당 인덱스부터 마지막 인덱스까지 모두 1씩 늘어난다.


따라서, 삽입이나 삭제 연산이 빈번히 일어나는 경우 ArrayList를 사용하는 것은 바람직 하지 않다.

**삽입이나 삭제 연산이 빈번히 일어나는 경우 [LinkedList](https://velog.io/@gillog/%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8Linked-List)를 사용하는 것이 좋다.**


<br>

## ArrayList 사용


ArrayList를 생성하기 위해서는 저장할 객체 타입을 타입 파라미터로 표기하고 아래와 같이 기본 생성자를 호출  하면 된다.

```java

// E는 제너릭이다.
List<E> arrayList = new ArrayList<E>();

```

**기본 생성자로 ArrayList를 생성하면 10개의 객체를 저장할 수 있는 초기 용량**을 가진다.

아래와 같이 **생성자의 매개값으로 용량의 크기를 지정**해 줄 수 도있다.

```java
// 생성시에 매개값으로 용량 20으로 크기 설정
List<E> arrayList = new ArrayList<E>(20);
```

아래는 ArrayList의 실제 메소드 사용 예제이다.


```java

// 생성시에 매개값으로 용량 20 설정
List<String> arrayList = new ArrayList<String>(20);
		
// data 삽입
arrayList.add("A");
		
// 삽입하고 싶은 인덱스에 데이터 삽입
arrayList.add(1,"B");
		
// 사이즈 얻기
int size = arrayList.size();

// 해당 인덱스의 객체 가져오기
String getStr = arrayList.get(1);
		
// 해당 인덱스의 객체 삭제
arrayList.remove(1);

// 인덱스 1의 객체를 "AA"로 변경
arrayList.set(1, "AA");
		
// 해당 객체가 존재하면 true를 return
boolean tr = arrayList.contains("A");
		
// list가 비었으면 true를 return
boolean fa = arrayList.isEmpty();
		
// list 비우기
arrayList.clear();
		
//제너릭 타입에 맞게 asList()의 매개값을 순차적으로 입력하거나,
List<String> list = Arrays.asList("가", "나");
		
//제너릭 타입의 배열을 매개값으로 주면 데이터를 삽입하며 생성 할 수 있다.
Integer [] intgerArray = {1,2,3};
List<Integer> list2 = Arrays.asList(intgerArray);

```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[클라우드스터딩-배열 리스트](https://cloudstudying.kr/lectures/138)

[배열 리스트 (ArrayList)[gdtbgl93님]](https://gdtbgl93.tistory.com/33)

[Data Structure(자료구조)[opentutorials]](https://opentutorials.org/module/1335/8709)

[[Java] 자바 ArrayList 사용법 & 예제 총정리[코딩팩토리]](https://coding-factory.tistory.com/551)