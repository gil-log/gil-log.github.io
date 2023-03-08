---
title: "[Coding Test] Programmers Level2 소수 찾기 (순열 조합, 소수)"
last_modified_at: 2020-12-05T03:46
categories: 
  - codingtest
tags: 
  - 'coding test' 
  - 'programmers' 
  - '소수 찾기' 
  - '순열' 
  - '조합'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Programmers Level2 소수 찾기
![](https://images.velog.io/images/gillog/post/8afdb588-bbdf-4ee1-885d-20086eb3e099/image.png)

## 문제 설명
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항
numbers는 길이 1 이상 7 이하인 문자열입니다.
numbers는 0~9까지 숫자만으로 이루어져 있습니다.
013은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

## 입출력 예

|numbers|	return|
|:--:|:--:|
|17|	3|
|011|	2|

## 입출력 예 설명
### 예제 #1
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.
### 예제 #2
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.
11과 011은 같은 숫자로 취급합니다.


# 풀이

1. 주어진 `numbers`를 하나 하나 쪼갠다.

2. 쪼개진 `number`들을 조합하여 숫자를 구성한다.
_011 = 11이므로 앞이 0으로 시작하는 조합은 제외, 조합되어 중복되는 수들은 하나만 사용_

3. `combination`된 `number`들의 소수 여부를 판별하여 소수의 개수를 구한다.

# 코드

```java
import java.util.Set;
import java.util.TreeSet;

class Solution {
    
    int answer = 0;
    Set<Integer> alredyExistsPrimeNumSet = new TreeSet<Integer>();
    
    public int solution(String numbers) {
        
        char[] numberChar = numbers.toCharArray();
        
        int length = numberChar.length;
        
        char[] combinationNumChar = new char[length];
        
        boolean[] used = new boolean[length];
        
        for(int i = 1; i <= length; i++){
            recursionCombination(numberChar, used, combinationNumChar, 0, length, i);
        }
        
        return answer;
    }
        
    public void recursionCombination(char[] numberChar, boolean[] used, char[] combinationNumChar, int count, int length, int maxCombination){
        if(count == maxCombination){
            if(isPrimeNumber(combinationNumChar, maxCombination))
                answer++;
            return;
        }
        
        for(int i = 0; i < length; i++){
            if(used[i])
                continue;
            
            used[i] = true;
            combinationNumChar[count] = numberChar[i];
            recursionCombination(numberChar, used, combinationNumChar, count + 1, length, maxCombination);
            used[i] = false;
        }
        
    }
    
    public boolean isPrimeNumber(char[] combinationNumChar, int maxCombination){
        
        if(combinationNumChar[0] == '0')
            return false;
        
        String combinationNumStr = "";
        
        for(int i = 0 ; i < maxCombination; i++){
            combinationNumStr += combinationNumChar[i];
        }
        
        int tempNumber = Integer.parseInt(combinationNumStr);
        
        if(tempNumber <= 1 || alredyExistsPrimeNumSet.contains(tempNumber))
            return false;
        
        for(int i = 2; i * i <= tempNumber; i++){
            if(tempNumber % i == 0)
                return false;
        }
        
        alredyExistsPrimeNumSet.add(tempNumber);
        
        return true;
    }
    
}
```

# 코드 풀이

1. 주어진 `String` `numbers`를 `String`의 `toCharArray()` method를 이용해 `char[]`로 바꾼다.
_`char[] numberChar` 생성_
2. `numbers`의 length를 구한다.

3. 쪼개진 숫자 문자를 조합할때 사용할 `char` `combincationNumChar[]` 배열을 생성한다.

4. 조합할때 해당 숫자를 사용했는지 여부를 가릴 `boolean` `used[]` 배열을 생성한다.

5. 중복되는 소수 카운팅을 제외하기 위해 `Set<Integer>` `alredyExistsPrimeNumSet`을 생성한다.
_데이터 생성에는 HashSet보다 느리지만, 잦은 검색에는 TreeSet이 더 효율적이므로 TreeSet 사용_

6. 재귀를 이용하여 숫자들을 조합할 `void` `recursionCombination` method를 생성, parameter로는 아래 변수들을 사용한다.
6.1 조합할 숫자 문자의 배열 `char[] numberChar`
6.2 조합에서 해당 숫자를 사용했는지 판단용도 `boolean[] used`
6.3 조합된 숫자 문자조합이 각각 담길 `char[] combinationNumChar`
6.4 해당 문자열의 최대 길이 `int length`
6.5 해당 조합에서 최대로 가능한 자리수를 나타내는 `int maxCombination`
6.6 해당 조합에서 현재 조합중인 자리수를 나타내는 `int count`

7. 소수 여부를 판별할 `boolean` `isPrimeNumber` method를 생성, parameter로는 아래 변수들을 사용한다.
7.1 조합된 숫자의 문자가 담긴 `char[] combinationNumChar`
7.2 해당 조합에서 최대로 가능한 자리수를 나타내는 `int maxCombination`

8. `solution`에서 `recursionCombination` method를 1부터 조합될 수 있는 문자열의 개수(`length`)만큼 반복한다.
_1부터 시작하는 이유는 1자리 수 부터 시작하기 위함_
8.1 `maxCombination`에 `i`를 넣음으로써 1자리 부터 주어진 `numbers`의 길이 만큼의 자리수 까지 숫자들을 조합하여 소수 여부를 판별한다.
8.2 해당 조합에서 현재 조합중인 자리수를 나타내는 `int count` 부분에 0을 넣는다.
8.3 `recursionCombination`에서는 `count`를 1개씩 늘려서 최대 `maxCombination`이 도달하면 지금까지 조합에 사용하기 위해 저장했던 `combinationNumChar`를 `isPrimeNumber`에 `maxCombination`와 함께 parameter로 사용한다.

9. `isPrimeNumber`에서 조합될 숫자를 조합하여 숫자로 변환 후 소수 여부를 판별한다.
9.1 조합될 첫 숫자가 `0`인 경우 `false`를 return한다.
9.2 `combinationNumChar[]`안에 숫자들을 for문으로 `String` `combinationNumStr`에 붙여준다.
9.2 `combinationNumStr`을 `int` `tempNumber`로 바꾼다.
9.3 `tempNumber`가 1이거나 이미 `answer`에서 카운팅된 소수들이 저장된 `alredyExistsPrimeNumSet`에 포함되면 `false`를 return한다.
9.4 `tempNumber`를 2부터 `tempNumber`의 제곱근까지 `%`연산 해보고 나누어 떨이지면 `false`를 return한다.
9.5 마지막 과정까지 나누어 떨어지지 않고 실행되었다면 `alredyExistsPrimeNumSet`에 `tempNumber`를 add한 후 `true`를 return한다.

10. `solution`에서 조합될 수 있는 문자열의 개수(`length`)만큼 반복 하던`recursionCombination` method가 종료된다.
11. `answer`에는 `recursionCombination` method에서 각 자리수의 최대 조합 가능 개수에서 `isPrimeNumber` method를 통해 true일 경우 `answer`의 값이 증가 하였으므로 `answer`를 return하여 답을 구한다.

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[프로그래머스] LEVEL 2 : 소수 찾기 (JAVA)[느리더라도 꾸준하게]](https://steady-coding.tistory.com/70)

