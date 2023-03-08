---
title: "[Python] Stack, Queue 기본, module 사용 정리"
last_modified_at: 2020-12-20T04:28
categories: 
  - python
tags: 
  - 'python' 
  - 'queue' 
  - 'stack'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[자료구조][파이썬/Python] 큐(Queue)[몽구의 우당탕탕 개발 공부]](https://mong9data.tistory.com/36?category=885885)

[Python에서의 스택과 큐[Jacoblog]](https://jacoblog.tistory.com/2)

[]()

[]()

[]()

[]()

<br>


# Stack

`Python`에서 내장 `Module`에는 stack만을 지원하는 기능은 없지만, 기본 `Python` `List`자료구조를 이용하면 아주 간단하게 `Stack`을 사용할 수 있다.

```python
stack = [0, 1, 2]

print(stack)

stack.append(3)

print(stack)

stack.pop()

print(stack)

'''
[0, 1, 2]
[0, 1, 2, 3]
[0, 1, 2]
'''
```
---

# Queue

`Python`에서 `Queue`는 **`List`를 이용해 구현**할 경우 **`dequeue`를 실행 할 때 시간 복잡도가 `O(1)`이 아닌 `O(N)`이 걸리는 문제**가 발생한다.

아래와 같이 앞에서 **추출한 데이터의 주소에 이전 데이터들을 이전하는 작업이 동반되어 시간 복잡도가 증가**하는 것이다.

```
 0x01    0x02     0x03

[  1  ] - [  2  ] - [  3  ]



 0x01    0x02    0x03

[Null] - [  2  ] - [  3  ]



 0x01    0x02    0x03

[  2  ] - [  3  ] - [  ?  ]
```

**양방향 연결 리스트로 `Queue`를 구현**하면 **`dequeue`를 실행 할 때 시간 복잡도가 `O(1)`**이 될 수 있지만, **`Queue` 중간 요소를 수정, 삭제, 삽입하는 경우 `List`로 구현한 `Queue`가 더 성능이 뛰어나다.**


그래서 **`Python` 내장 `Module`로 `Queue`를 지원하는 `Queue` `Module`**이 있지만, 이는 **`Thread`환경을 위해 만들어져있어 일반적인 상황에서 사용은 권장되지 않는다.**

<br>

그래서** 보통 `Collections`의 `deque` `Module`을 사용**한다.


## collections.deque

**`collections.deque`의 `deque`**는 double ended queue의 약자로, **양방향 연결리스트(Doubly Linked List)로 구성**되어 있다.


```python
from collections import deque

d = deque()
print(type(d))

# <class 'collections.deque'>
```

### deque.append : enqueue 기능 수행

```python
from collections import deque

d = deque()

for i in range(5):
    d.append(i)
print(d)

# deque([0, 1, 2, 3, 4])
```

### deque.appendleft : 왼쪽에 데이터 삽입

```python
from collections import deque

d = deque()
for i in range(5):
    d.append(i)

d.appendleft(10)
print(d)

# deque([10, 0, 1, 2, 3, 4])
```

### deque.insert : Queue 중간에 데이터 삽입

```python
from collections import deque

d = deque()
for i in range(5):
    d.append(i)

d.insert(4, 11)
print(d)

# deque([0, 1, 2, 3, 11, 4])
```

### deque.extend, deque.extendleft : Queue 오른쪽, 왼쪽에 iterable 객체 append 연장


```python
from collections import deque

d = deque()
for i in range(5):
    d.append(i)
    
d.extend([0, 0, 0])
print(d)
d.extendleft([0, 0, 0])
print(d)

# deque([0, 1, 2, 3, 11, 4, 0, 0, 0])
# deque([0, 0, 0, 0, 1, 2, 3, 11, 4, 0, 0, 0])
```

### deque.popleft : 왼쪽 데이터 제거 후 return

```python
from collections import deque

d = deque()
for i in range(5):
    d.append(i)

print(d.popleft())

# 0
```

### deque.pop : 오른쪽 데이터 제거 후 return

```python
from collections import deque

d = deque()
for i in range(5):
    d.append(i)

print(d.pop())

# 4
```

### list(deque) : deque 역시 list로 사용할 수 있다.

```python
from collections import deque

d = deque()
for i in range(5):
    d.append(i)

list(d)

print(d)

# [0, 1, 2, 3, 4]
```