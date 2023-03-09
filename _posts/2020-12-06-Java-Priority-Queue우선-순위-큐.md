---
title: "[Java] Priority Queue(우선 순위 큐)"
last_modified_at: 2020-12-06T00:42
categories: 
  - data-structure
tags: 
  - 'PriorityQueue' 
  - '우선 순위 큐' 
  - '자료구조'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Priority Queue

**`PriorityQueue`**란 **`우선순위 큐`**로써 **일반적인 큐의 구조 FIFO(First In First Out)를 가지면서**, 데이터가 **들어온 순서대로 데이터가 나가는 것이 아닌 우선순위를 먼저 결정**하고 그 **우선순위가 높은 데이터가 먼저 나가는 자료구조**이다.

**`PriorityQueue`를 사용하기 위해선** 우선순위 큐에 **저장할 객체는 필수적으로 `Comparable Interface`를 구현해야한다.**

**`Comparable Interface`를 구현하면 `compareTo` method를 override** 하게 되고 해당 **객체에서 처리할 우선순위 조건을 리턴**해주면 **`PriorityQueue` 가 알아서 우선순위가 높은 객체를 추출** 해준다. 


**`PriorityQueue`는 `Heap`을 이용하여 구현하는 것이 일반적**이다. 

**데이터를 삽입할 때 우선순위를 기준으로 최대 힙 혹은 최소 힙을 구성**하고 **데이터를 꺼낼 때 루트 노드를 얻어낸 뒤** 루트 노드를** 삭제할 때는 빈 루트 노드 위치에 맨 마지막 노드를 삽입**한 후 **아래로 내려가면서 적절한 자리를 찾아 옮기는 방식으로 진행**된다.
_최대 값이 우선순위인 큐 = 최대 힙, 최소 값이 우선순위인 큐 = 최소 힙_


# Priority Queue 특징

1. **높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조**이다.
_우선순위 큐에 들어가는 원소는 비교가 가능한 기준이 있어야한다._

2. **내부 요소는 힙으로 구성되어 이진트리 구조**로 이루어져 있다. 

3. 내부구조가 **힙으로 구성되어 있기에 시간 복잡도는 `O(NLogN)`**이다.

4. **우선순위를 중요시해야 하는 상황에서 주로 쓰인다.**


# Priority Queue 선언

`Priority Queue`를 사용하려면 `java.util.PriorityQueue`를 import해야 한다.

```java
import java.util.PriorityQueue;
import java.util.Collections;

//낮은 숫자가 우선 순위인 int 형 우선순위 큐 선언
PriorityQueue<Integer> priorityQueueLowest = new PriorityQueue<>();

//높은 숫자가 우선 순위인 int 형 우선순위 큐 선언
PriorityQueue<Integer> priorityQueueHighest = new PriorityQueue<>(Collections.reverseOrder());
```

# Priority Queue 동작

## Priority Queue Add

```java
// add(value) 메서드의 경우 만약 삽입에 성공하면 true를 반환, 
// 큐에 여유 공간이 없어 삽입에 실패하면 IllegalStateException을 발생
priorityQueueLowest.add(1);
priorityQueueLowest.add(10);
priorityQueueLowest.offer(100);

priorityQueueHighest.add(1);
priorityQueueHighest.add(10);
priorityQueueHighest.offer(100);
```

`Priority Queue`에 삽입이 실행되면 아래와 같은 구조로 데이터의 삽입이 이루어 진다.

![](https://images.velog.io/images/gillog/post/7645823c-565c-4ee0-ae47-c8332ef0bc7c/img1.daumcdn.png)

> 그림 출처 : [[Java] PriorityQueue(우선순위 큐) 클래스 사용법 & 예제 총정리[코딩팩토리]](https://coding-factory.tistory.com/603)

## Priority Queue remove

```java
// 첫번째 값을 반환하고 제거 비어있다면 null
priorityQueueLowest.poll();

// 첫번째 값 제거 비어있다면 예외 발생
priorityQueueLowest.remove(); 

// 첫번째 값을 반환만 하고 제거 하지는 않는다.
// 큐가 비어있다면 null을 반환
priorityQueueLowest.peek();

// 첫번째 값을 반환만 하고 제거 하지는 않는다.
// 큐가 비어있다면 예외 발생
priorityQueueLowest.element();

// 초기화
priorityQueueLowest.clear();      
```
`Priority Queue`에 삭제는 아래와 같은 구조로 이루어 진다.

![](https://images.velog.io/images/gillog/post/1bcea67b-80a9-437a-8a8d-cb55b41adc54/img1.daumcdn.png)

> 그림 출처 : [[Java] PriorityQueue(우선순위 큐) 클래스 사용법 & 예제 총정리[코딩팩토리]](https://coding-factory.tistory.com/603)


# Priority Queue Using Costom Class

`Priority Queue`안에 담길 객체를 자신만의 Class로 사용하려면 **`Comparable Interface`를 implements하는 Class를 생성**한 후, **`compareTo` method를 우선 순위에 맞게 구현**해 주면 된다.

## Costom Class

```java
class Gillog implements Comparable<Gillog> {

    private int writeRowNumber;
    private String content;

    public Gillog(int writeRowNumber, String content) {
        this.writeRowNumber = writeRowNumber;
        this.content = content;
    }

    public int getWriteRowNumber() {
        return this.writeRowNumber;
    }

    public String getContent() {
        return this.content;
    }

    @Override
    public int compareTo(Gillog gillog) {

        if (this.writeRowNumber > gillog.getWriteRowNumber())
            return 1;
        else if (this.writeRowNumber < gillog.getWriteRowNumber())
            return -1;
        return 0;
    }
}

```


## Priority Queue에 적용
```java
    public static void main(String[] args) {

        PriorityQueue<Gillog> priorityQueue = new PriorityQueue<>();

        priorityQueue.add(new Gillog(3650, "10년후 글"));
        priorityQueue.add(new Gillog(31, "한달 후 글"));
        priorityQueue.add(new Gillog(1, "첫번째 글"));
        priorityQueue.add(new Gillog(365, "1년후 글"));

        while (!priorityQueue.isEmpty()) {
            Gillog gilLog = priorityQueue.poll();
            System.out.println("글 넘버 : " + gilLog.getWriteRowNumber() + " 글 내용 : " + gilLog.getContent());
        }
    }
    
 /* 실행 결과
글 넘버 : 1 글 내용 : 첫번째 글
글 넘버 : 31 글 내용 : 한달 후 글
글 넘버 : 365 글 내용 : 1년후 글
글 넘버 : 3650 글 내용 : 10년후 글
 */
```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[Java] PriorityQueue(우선순위 큐) 클래스 사용법 & 예제 총정리[코딩팩토리]](https://coding-factory.tistory.com/603)

[자바로 정리한 우선순위큐(PriorityQueue)[팡스 블로그]](https://pangsblog.tistory.com/23)


[Java Heap & Priority Queue[agugu95.log]](https://velog.io/@agugu95/Java-Heap-Binary-Heap)

[]()

[]()
