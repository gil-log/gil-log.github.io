---
title: "버블 정렬(Bubble Sort)"
last_modified_at: 2020-10-26T23:52
categories: 
  - algorithm
tags: 
  - '버블 정렬' 
  - '알고리즘' 
  - '정렬'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 버블 정렬(Bubble Sort)

**`Bubble Sort`**는 **Selection Sort와 유사한 알고리즘**으로 **서로 인접한 두 원소의 대소를 비교**하고, **조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘**이다.


![](https://images.velog.io/images/gillog/post/9f3ab7d6-54a3-468f-b892-84710575cfe7/bubble-sort-001.gif)


# 동작 과정

1회전에 첫 번째 원소와 두 번째 원소를, 두 번째 원소와 세 번째 원소를, 세 번째 원소와 네 번째 원소를, … 이런 식으로 (마지막-1)번째 원소와 마지막 원소를 비교하여 **이전 원소가 더 큰 경우 서로 교환**한다.

1회전을 수행하고 나면 **가장 큰 원소가 맨 뒤로 이동**하므로 **2회전에서는 맨 끝에 있는 원소는 정렬에서 제외**되고, 2회전을 수행하고 나면 끝에서 두 번째 원소까지는 정렬에서 제외된다. 

이렇게 **정렬을 1회전 수행할 때마다 정렬에서 제외되는 데이터가 하나씩 늘어난다.**







# 시간, 공간 복잡도

## 시간 복잡도
시간복잡도를 계산하면, (n-1) + (n-2) + (n-3) + .... + 2 + 1 => n(n-1)/2이므로, **`O(n^2)`** 이다.

또한,**Bubble Sort는 중간에 정렬이 완료되었음에도, 마지막 회차까지 2개의 원소를 비교하기 때문**에 **최선, 평균, 최악의 경우 모두 시간복잡도가 O(n^2) 으로 동일**하다.

Bubble Sort는 성능이 좋지 않으나 **코드가 단순하여 간단한 데이터의 정렬에는 사용되기도 한다**.


## 공간 복잡도
**주어진 배열 안**에서 **교환(swap)을 통해, 정렬이 수행**되므로 **`O(n)`**이다.

# 버블 정렬 구현


```java

    public static void main(String[] args) {

        int[] arrays = { 6, 32, 36, 49, 43, 29, 19, 28, 16, 41, 45 };

        System.out.println(Arrays.toString(bubbleSort(arrays)));
    }

    static int[] bubbleSort(int[] arr) {
        // 임시 저장용도
        int temp = 0;
        // 연산 횟수 카운트 용도
        int count = 0;

        for (int i = 0; i < arr.length; i++) {

            // 점차 실행 될때마다 마지막 항은 제일 큰 값이므로 연산하지 않음
            for (int j = 1; j < arr.length - i; j++) {
                // 이전 인덱스가 현재 인덱스보다 값이 큰 경우 스왑 발생
                if (arr[j - 1] > arr[j]) {
                    // temp에 이전 인덱스 값 저장
                    temp = arr[j - 1];
                    // 현재 값 이전 인덱스로 스왑
                    arr[j - 1] = arr[j];
                    // 이전 값 현재 인덱스로 스왑
                    arr[j] = temp;
                }
                
                // 연산 횟수 카운트
                count++;
            }
        }

        System.out.println("연산 수행 횟수는 : "+ count);
        return arr;
    }
```


- 실행 결과
```
연산 수행 횟수는 : 55
[6, 16, 19, 28, 29, 32, 36, 41, 43, 45, 49]
```

# 개선된 버블 정렬

Bubble Sort를 진행하다 보면 **특정 회차에서 정렬이 안료되었음에도,** 위 코드에서처럼 **마지막 회차까지 비교 연산을 진행해야한다.**

이를 개선하기 위해 서는 만약 **중간 회차에서 데이터의 swap 과정이 진행되지 않으면, 정렬이 완료되었다는 것이므로 연산을 종료**하는 코드를 넣어주면 된다.


```java

    public static void main(String[] args) {

        int[] arrays = { 6, 32, 36, 49, 43, 29, 19, 28, 16, 41, 45 };

        System.out.println(Arrays.toString(bubbleSort(arrays)));
    }

    static int[] bubbleSort(int[] arr) {
        // 임시 저장용도
        int temp = 0;
        // 특정 회차 정렬 완료 확인 용도 flag
        int doneFlag = 1;
        // 연산 횟수 카운트 용도
        int count = 0;

        for (int i = 0; i < arr.length; i++) {
            // swap 연산이 수행되는지 확인 하기 위해 flag를 킨다.
            doneFlag = 1;

            // 점차 실행 될때마다 마지막 항은 제일 큰 값이므로 연산하지 않음
            for (int j = 1; j < arr.length - i; j++) {
                // 이전 인덱스가 현재 인덱스보다 값이 큰 경우 스왑 발생
                if (arr[j - 1] > arr[j]) {
                    // temp에 이전 인덱스 값 저장
                    temp = arr[j - 1];
                    // 현재 값 이전 인덱스로 스왑
                    arr[j - 1] = arr[j];
                    // 이전 값 현재 인덱스로 스왑
                    arr[j] = temp;

                    // swap 연산이 수행되었다는 확인을 하고 flag를 끈다.
                    doneFlag = 0;
                }
                
                // 연산 횟수 카운트
                count++;
            }

            // swap 연산이 한 번도 수행되지 않았으면 
            // 정렬이 done 되었으므로 연산을 종료한다.
            if (doneFlag == 1)
                break;
        }

        System.out.println("연산 수행 횟수는 : "+ count);
        return arr;
    }
```

- 실행 결과
```
연산 수행 횟수는 : 52
[6, 16, 19, 28, 29, 32, 36, 41, 43, 45, 49]
```

실행 결과처럼 특정 회차에서 정렬이 완료되면 연산이 종료되므로 연산 수행 횟수가 줄어들었다.

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[신입 개발자 전공 지식 & 기술 면접 백과사전](https://github.com/GimunLee/tech-refrigerator/blob/master/Algorithm/%EA%B1%B0%ED%92%88%20%EC%A0%95%EB%A0%AC%20(Bubble%20Sort).md#%EA%B1%B0%ED%92%88-%EC%A0%95%EB%A0%AC-bubble-sort)

[버블 정렬(Bubble sort)에 대하여!](https://m.blog.naver.com/tipsware/221297715324)

[]()

[]()

[]()

[]()
