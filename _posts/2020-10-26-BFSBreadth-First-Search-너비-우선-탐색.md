---
title: "BFS(Breadth First Search, 너비 우선 탐색)"
last_modified_at: 2020-10-26T01:27
categories: 
  - 알고리즘
tags: 
  - 'BFS' 
  - '알고리즘' 
  - '탐색'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---



# BFS(너비 우선 탐색)


BFS(Breadth First Search, 너비 우선 탐색)란 루트 노드(혹은 임의 노드)에서 시작해서 인접한 노드들 부터 탐색하는 방법이다.

**시작 노드로부터 가까운 노드를 먼저 방문**하고 **멀리 떨어져 있는 노드를 나중에 방문하는 탐색 방법**이다.

즉, 아래 사진처럼 깊게(deep) 탐색하기 전에 **넓게(wide) 탐색하는 것**이다.


![](https://images.velog.io/images/gillog/post/334c47ef-56c3-426d-ba89-9a17845393d8/img1.daumcdn.gif)

멀리 떨어진 노드는 나중에 탐색하는 방식이기 때문에 **두 노드 사이의 최단 경로 혹은 임의의 경로를 찾고 싶을 때 이 방법을 선택**한다.

**BFS는 큐를 이용해 구현**하며, **노드를 방문할 때마다, 이웃 노드들을 enqueue**하여 담아놓고 **저장된 순서대로 탐색하여 차례대로 dequeue** 하는 방식이다.


실제 동작 방식은 아래와 같다.

_원본 출처 : [알고리즘 Graph - BFS 너비 우선 탐색[Manducku`s Code님]](https://manducku.tistory.com/24?category=683258)_



![](https://images.velog.io/images/gillog/post/4fa4ae5c-87aa-4f16-8248-7b7318287681/2464AD4E562A59611E.png)

위 그래프를 노드1 부터 BFS로 탐색 하면 아래와 같다.

![](https://images.velog.io/images/gillog/post/68f564db-7059-462c-ac6b-fc9a1418072d/2463104E562A59621F.png)

먼저 최초로 방문할 노드1을 큐에 enqueue한다.

![](https://images.velog.io/images/gillog/post/c3b2b315-2629-4cf0-897a-7f150b78c0d1/21519745562A596302.png)

방문할 값을 찾기 위해 큐에서 dequeue하여 노드1을 꺼내어 방문하고, 노드1과 인접한 노드5, 6을 큐에 enqueue한다.

![](https://images.velog.io/images/gillog/post/c8e99966-5a02-4775-a056-04d784218337/2645FE41562A59642D.png)

다시 방문할 값을 찾기 위해 큐에서 dequeue하여 노드5를 꺼내어 방문하고, 노드5와 인접한 노드3을 큐에 enqueue한다.
_여기서 노드6은 기존에 큐에 있는 데이터이므로 중복해서 삽입하지 않는다._
_hash or visit[boolean]에 값 체크 등으로 중복 방지_

![](https://images.velog.io/images/gillog/post/2f6d4627-ec83-496e-9500-bf92594a96b6/2769E036562A596605.png)

다시 방문할 값을 찾기 위해 큐에서 dequeue하여 노드6을 꺼내어 방문하고, 노드6과 인접한 노드2를 큐에 enqueue한다.
_노드5는 이미 방문한 노드이므로 삽입하지 않는다._


![](https://images.velog.io/images/gillog/post/9e020028-9e52-4383-81b6-1b129d07cb00/235BFD33562A596835.png)

다시 방문할 값을 찾기 위해 큐에서 dequeue하여 노드3을 꺼내어 방문하고, 노드3과 인접한 노드2, 4를 큐에 enqueue한다.
_노드 5는 이미 방문한 노드이므로 삽입하지 않는다._

![](https://images.velog.io/images/gillog/post/b33582df-14d8-4738-9e13-2a28ea0c6d0e/21795933562A596A1A.png)

다시 방문할 값을 찾기 위해 큐에서 dequeue하여 노드2를 꺼내어 방문한다.
_노드 3, 6은 이미 방문한 노드이고 노드 4는 이미 큐에 있는 데이터 이므로 아무것도 enqueue하지 않는다._

![](https://images.velog.io/images/gillog/post/9cb1cdd1-0bd7-498c-a474-307f83f9b8e3/2762234A562A596B13.png)
마지막으로 큐에있는 노드4를 방문하고 모든 인접 노드가 이미 방문했고, 큐에 데이터가 없으므로 탐색을 종료한다.







# BFS 장, 단점

## 장점
- **노드의 수가 적고 깊이가 얕은 경우 빠르게 동작**할 수 있다.
- **단순 검색 속도가 깊이 우선 탐색(DFS)보다 빠르다.**
- 너비를 우선 탐색하기에 **답이 되는 경로가 여러개인 경우에도 최단경로임을 보장**한다.
- **최단경로가 존재한다면 어느 한 경로가 무한히 깊어진다해도 최단경로를 반드시 찾을 수 있다.**

 

## 단점
- 재귀호출의 DFS와는 달리 **큐에 다음에 탐색할 정점들을 저장해야 하므로 저장공간이 많이 필요**하다.
- **노드의 수가 늘어나면 탐색해야 하는 노드 또한 많아지기에 비현실적**이다.


# BFS 시간 복잡도

|인접 행렬 구현|인접 리스트 구현|
|:----:|:----:|
|O( N^2 )|O( N + E )|



# BFS 인접 리스트 구현


```java
static int Ne; // 간선 개수
static int Nv; // 정점(노드) 개수
static ArrayList<ArrayList<Integer>> ad; //인접 리스트
static boolean[] visit; // 정점(노드) 방문 여부 확인 용도

public static void bfs(int i){
    Queue <Integer>q = new <Integer> LinkedList();
    // hash Map을 이용하여 queue data 중복 방지
    HashMap<Integer, Boolean> hash = new HashMap<Integer, Boolean>();
    
    // add는 큐가 꽉차면 예외 발생, 
    // offer는 추가에 실패를 의미하는 false return
    q.offer(i);
    
    //q가 빌때 까지 실행
    while(!q.isEmpty()){
        // 방문할 노드를 큐에서 값 빼기, 
        // remove는 큐에 데이터가 없으면 예외발생, 
        // poll은 데이터가 없으면 실패를 의미하는 false return
        int temp = q.poll();
        // 해당 노드를 방문 처리
        visit[temp] = true;
        // 방문 노드 출력
        System.out.print(temp);
        
        // arraylist에서 방문할 노드에서 인접한 노드들 arraylist 값 하나 하나 출력
        for(int j : ad.get(temp)){
            // 인접 노드에 방문하지 않았고, 중복된 데이터를 입력한 적 없으면
            if(visit[j] == false && !hash.containsKey(j)){
                // 다음 방문 노드로 큐에 넣기
                q.offer(j);
                // hash에 j 방문 정보 입력
                hash.put(j, true);
            }
        }
    }
}

public static void main(String[] args) {
    Nv = 8;
    Ne = 6;
    ad = new <ArrayList<Integer>> ArrayList(Nv+1);
    visit = new boolean[Nv+1];
    
    for(int i = 0; i <= Nv+1; i++){
        ad.add(new ArrayList());
    }

    put(1,5);
    put(1,6);
    put(2,3);
    put(2,4);
    put(2,6);
    put(3,4);
    put(3,5);
    put(5,6);
    
    bfs(1);        
}

    // 그래프 추가 (양방향)
public static void put(int x, int y) {
    ad.get(x).add(y);
    ad.get(y).add(x);
}
```

- 실행 결과

```
156324
```



# BFS 인접 행렬 구현


```java
static int Ne; // 간선 개수
static int Nv; // 정점(노드) 개수
static int[][] ad; // 인접 배열
static boolean[] visit; // 정점(노드) 방문 여부 확인 용도

public static void bfs(int i) {
    Queue<Integer> q = new <Integer>LinkedList();
    HashMap<Integer, Boolean> hash = new HashMap<Integer, Boolean>(); // hash Map을 이용하여 queue data 중복 방지

    // add는 큐가 꽉차면 예외 발생, 
    //offer는 추가에 실패를 의미하는 false return
    q.offer(i);

    while (!q.isEmpty()) {
        // 방문할 노드를 큐에서 값 빼기, 
        // remove는 큐에 데이터가 없으면 예외발생, 
        // poll은 데이터가 없으면 실패를 의미하는 false return
        int temp = q.poll();
        // 해당 노드를 방문으로 처리
        visit[temp] = true;
        // 방문 노드 출력
        System.out.print(temp);

        // 노드는 1부터 존재하므로 1부터 시작
        // 이중 배열 생성 당시 크기를 Nv+1 해주어서 문제 없음
        for (int j = 1; j <= Nv; j++) {
            // 방문 노드에 연결된 노드이고 방문하지않은 노드 일 경우
            if (ad[temp][j] == 1 && visit[j] == false) {
                // 중복 데이터가 hash에 없을경우
                if (!hash.containsKey(j)) {
                    // q에 노드j 삽입
                    q.offer(j);
                    // hash에 j 방문 정보 입력
                    hash.put(j, true);
                }
            }
        }
    }
}

public static void main(String[] args) {
    Ne = 8;
    Nv = 6;

    // 노드는 1부터 시작이라 노드 개수 + 1
    ad = new int[Nv + 1][Nv + 1];
    visit = new boolean[Nv + 1];

    int[][] arr = {
        {0, 0, 0, 0, 0, 0, 0},
        {0, 0, 0, 0, 0, 1, 1},
        {0, 0, 0, 1, 1, 0, 1},
        {0, 0, 1, 0, 1, 1, 0},
        {0, 0, 1, 1, 0, 0, 0},
        {0, 1, 0, 1, 0, 0, 1},
        {0, 1, 1, 0, 0, 1, 0}
    }; 

    ad = arr;
    
    bfs(1);
}

```

- 실행 결과

```
156324
```



<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[알고리즘] 너비 우선 탐색(BFS)이란](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)

[알고리즘 개념 - 너비우선탐색(BFS)](https://velog.io/@sukong/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%9C%EB%85%90-%EB%84%88%EB%B9%84%EC%9A%B0%EC%84%A0%ED%83%90%EC%83%89BFS-lp8zywtn)

[[Algorithm] BFS 알고리즘 (Breadth-First Search)](https://coding-factory.tistory.com/612)

[알고리즘 Graph - BFS (너비 우선 탐색)](https://manducku.tistory.com/24?category=683258)

[]()

[]()
