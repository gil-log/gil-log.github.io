---
title: "Collections 클래스"
last_modified_at: 2020-10-23T02:23
categories: 
  - java
tags: 
  - 'Java' 
  - 'collections'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# Collections 클래스

**`Collections 클래스`**는 **Collection 인터페이스를 구현한 클래스에 대한 객체생성, 정렬(sort), 병합(merge), 검색(search) 등의 기능을 안정적으로 수행하도록 도와주는 역할**을 하는 유틸리티 클래스이다.

이 메소드들은 제네릭 기술을 사용하여 작성되었으며 정적 메소드의 형태로 되어있다.

자주 사용되는 알고리즘으로는 `정렬(Sorting)`, `섞기(Shuffling)`, `탐색(Searching)` 등이 있다. 


# 주요 method()

|메소드|설명|
|:----:|:----:|
|max|지정된 컬렉션에서 최대 요소를 반환한다. (인덱스  X)|
|min|지정된 컬렉션에서 초소 요소를 반환한다. (인덱스 X)|
|sort|지정된 컬렉션을 정렬시킨다. <br>오버로드 메소드들이 존재하며 가장 기본적인 메소드는 자연순서에 따라 내림차순으로 정렬된다.|
|shuffle|지정된 컬렉션의 요소들의 순서를 무작위로 섞는다.|
|synchronizedCollection|지정된 컬렉션에 의해 지원되는 동기화 된 컬렉션을 재생성해 반환한다.|
|binarySearch|지정된 컬렉션에서 이진 탐색 알고리즘을 사용해 지정된 객체를 검색해 인덱스를 반환한다.|
|disjoint|2개의 지정된 컬렉션들에서 공통된 요소가 하나도 없는 경우 true 를 반환한다.|
|copy|지정된 켈렉션의 모든 요소를 새로운 컬렉션으로 복사해 반환한다.|
|reverse|지정된 컬렉션에 있는 순서를 역으로 변경한다.|

# sort()

Collections 클래스의 정렬은 속도가 비교적 빠르고 안전성이 보장되는 합병 정렬을 이용한다.

안전성이란 동일한 값을 가지는 원소를 다시 정렬하지 않는 것을 의미한다.

안전성은 같은 리스트를 반복하여 다른 기준에 따라 정렬할 때 중요하다.

예를 들어 상품 주문 리스트를 날짜를 기준으로 먼저 정렬하고 이후 주문처를 기준으로 정렬한다면 사용자는 같은 주문처가 보낸 주문은 날짜별로 정렬될 것이라고 가정한다. 이처럼 한번 정렬된 것이 유지되는 경우를 안정성 있는 정렬이라고 말한다.

Collection 클래스의 sort() 메소드는 정적메소드로, List 인터페이스를 구현하는 컬렉션에 대하여 정렬을 수행한다.
-_ **Collection.sort() 는 list,   Arrays.sort() 는 array 에서 사용**한다._

```java
//원소가 String 타입이므로 알파벳 순서대로 정렬.

List<String> list = new LinkedList<String>();
list.add(“철수”);
list.add(“영희”);
Collections.sort(list);

```

원소가 Date 타입이라면 시간적인 순서로 정렬될 것이다. 
그 이유는 String 클래스와 Date 클래스 모두 Comparable 인터페이스를 구현하기 때문이다.

정렬은 Comparable 인터페이스를 이용하여 이루어진다. List 인터페이스는 Comparable 인터페이스를 상속하므로 정렬을 이용 할 수 있다.
```java
public interface Comparable<T> {
public int comparaTo(T o);
}
```

comparaTo() 메소드는 매개변수 객체를 현재의 객체와 비교하여 작으면 음수, 같으면 0, 크면 양수를 반환한다.


# shuffle()

리스트에 존재하는 정렬을 파괴하여서 원소들의 순서를 랜덤하게 만든다.
정렬과는 반대 동작을 한다.

```java
// 철수와 영희의 순서가 섞이게 된다.
List<String> list = new LinkedList<String>();
list.add(“철수”);
list.add(“영희”);
Collections.shuffle(list);
```


# binarySearch()

Collections 클래스는 정렬된 리스트에서 지정된 원소를 이진 탐색한다. 
binarySearch() 메소드는 **“정렬된 리스트”에 한해서 탐색**을 진행한다는 점에 주의하여야 한다.

> 선형 탐색 : 처음부터 모든 원소를 방문하는 탐색 방법으로, 리스트가 정렬되어 있지 않은 경우 사용한다.
이진 탐색 : 리스트가 정렬되어 있는 경우 중간에 있는 원소(n)와 먼저 비교하여, 크면 그 다음부터(n+1) 끝까지 비교하고, 작으면 처음부터 그 전(n-1)까지의 원소들과 비교하는 방식을 반복하여 하나의 리스트를 계속해서 두 개의 리스트로 분할한다. 
이 방법은 리스트에 하나의 원소가 남을 때까지 반복된다.** 이진 탐색은 문제의 크기를 반으로 줄일 수 있기 때문에 선형 탐색보다 효율적**이다.

```java
// list는 리스트, element는 탐색할 원소이다.
int index = Collections.binarySearch(list, element);

```
이진 탐색 메소드인 binarySearch()는 만약 반환값이 양수이면 찾고자 하는 원소의 인덱스값을 출력한다.
음수이면 탐색이 실패하여 원소를 찾지 못했음을 의미한다.

단, **binarySearch() 메소드는 실패하더라도 도움이 되는 정보를 반환**한다. 
즉, 반환값에는 **현재의 데이터가 삽입될 수 있는 위치 정보**가 있다. 
반환값이 **pos라고 한다면, (-pos-1)이 해당 데이터를 삽입할 수 있는 위치**이다.

```java
// -pos-1이 데이터 삽입 가능 index이다.
int pos = Collections.binarySearch(list, key);
if(pos < 0) list.add(-pos-1);
```
 


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[42. Collection Class - 컬렉션 클래스](https://movefast.tistory.com/80)

[Collections 클래스 활용](https://ssg4089.tistory.com/10)

[java.util.Collections #](http://www.incodom.kr/Java/java.util.Collections)

[]()

[]()

[]()