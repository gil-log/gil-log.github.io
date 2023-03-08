---
title: "gil.getPyCheatSheet();"
last_modified_at: 2020-12-12T07:09
categories: 
  - gillibraryutils
tags: 
  - 'Cheat Sheet' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 숫자 입력 공백 쪼개서 list 넣기

```python
number_list = list(map(int, input().split()))
```

# exit()
프로그램 종료 함수
_예외상황 빼두고 exit()로 종료 시키기도 가능_

# id()
Python 내장 함수로 객체 주소 return 함수

# type()

Python 내장 함수로 객체 타입 출력 함수

# try except

```python
while True :
    try:
        A, B = map(int, input().split())
        print(A+B)
    except:
        break
        
try:
    while True:
        A, B = map(int, input().split())
        print(A+B)
except:
    print("error")
    break
```

# print

```python
number = int(input())

for multipleNumber in range(1, 10):
    printNum = int(number * multipleNumber)
    print(number, "*", multipleNumber, "=", printNum)

'''
실행결과
2
2 * 1 = 2
2 * 2 = 4
2 * 3 = 6
2 * 4 = 8
2 * 5 = 10
2 * 6 = 12
2 * 7 = 14
2 * 8 = 16
2 * 9 = 18
'''
```

- % 활용 "문자열%d" %(dddd)

```python
testCaseT = int(input())

for testCase in range(1, testCaseT+1):
    A, B = map(int, input().split())
    print("Case #%d: %s" %(testCase, A+B))
```

- "%.3f"%실수인값 > 소숫점 3자리까지 표현

```python
 print("%.3f"%aboveAverageStudentRate+"%")
 # 40.000%
```

# input

- 공백으로 입력 받은 수 받아오기

```python
a, b = map(int, input().split())
```

# import
```python
import sys

testCaseT = int(input())

for testCase in range(testCaseT):
    A, B = map(int, sys.stdin.readline().rstrip().split())
    print(A+B)
```
_input 대신 sys.stdin.readline을 사용할 수 있다. 
단, 이때는 맨 끝의 개행문자까지 같이 입력받기 때문에 문자열을 저장하고 싶을 경우 .rstrip()을 추가로 해 주는 것이 좋다._


# for

- i in range(i, 0, -1)
역순 감소 for문


```python
N = int(input())

for i in range(N, 0, -1):
    print(i)
```


```python

```

```python

```

```python

```
