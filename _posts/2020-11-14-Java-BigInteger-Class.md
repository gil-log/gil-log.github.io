---
title: "[Java] BigInteger Class"
last_modified_at: 2020-11-14T02:50
categories: 
  - java
tags: 
  - 'BigInteger' 
  - 'Java' 
  - 'class'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# BigInteger Class

**`BigInteger Class`**는 Java에서 **정수형의 기본 자료형 `int`, `long`** 의 **저장 가능한 값의 크기를 넘어서는** **정수형을 저장할 수 있는 Class**이다.

**`long`은 약 10^18승(최대 값 9223372036854775807) 범위까지 수를 표현**한다. 

**이 범위를 넘어갈 때**는 Java에서 제공하는 **BigInteger Class를 이용**하면 된다.

**`BigInteger`는 문자열 형태**로 이루어져 있어 **숫자 범위가 무한하기에 어떠한 숫자 데이터도 담을 수 있다.**

`BigInteger`는 내장 Class가 아니라, `math package`에 포함되어 있다.

package 구조는 `java.math.BigInteger`이다.




# BigInteger 사용 예제

```java


import java.math.BigInteger;

public class BigIntegerStudy {
    public static void main(String[] args) {

        // long 최대 범위 9223372036854775807 을 넘어서 저장 가능.
        BigInteger bigInt = new BigInteger("9223372036854775808");

        // 9진법 7777을 10진법으로 변환
        // 출력 결과 : 5740
        BigInteger radixBigInt = new BigInteger("7777", 9);

        // BigInteger.intValue();
        // BigInteger를 int로 형변환 한다.
        int intBigNum = radixBigInt.intValue();

        // BigInteger.longValue();
        // BigInteger를 long으로 형변환 한다.
        long longBigNum = radixBigInt.longValue();

        // BigInteger.floatValue();
        // BigInteger를 float으로 형변환 한다.
        float floatBigNum = radixBigInt.floatValue();

        // BigInteger.doubleValue();
        // BigInteger를 double로 형변환 한다.
        double doubleBigNum = radixBigInt.doubleValue();

        // BigInteger.toString();
        // BigInteger를 String으로 return한다.
        // 출력 결과 : "9223372036854775808"
        String strBigInt = bigInt.toString();

        // BigInteger.min(BigInteger);
        // BigInteger 2개 중 작은 값을 return한다.
        // 출력 결과 : 5740
        BigInteger minBigInt = bigInt.min(radixBigInt);

        // BigInteger.max(BigInteger);
        // BigInteger 2개 중 큰 값을 return한다.
        // 출력 결과 : 9223372036854775808
        BigInteger maxBigInt = bigInt.max(radixBigInt);

        // BigInteger.compareTo(BigInteger);
        // BigInteger을 앞, 뒤 2가지 값을 비교하여
        // 앞이 크면 1, 같으면 0, 작으면 -1을 int로 return한다.
        // 출력 결과 : 1
        int compareToBigInt = bigInt.compareTo(radixBigInt);

        // BigInteger.add(BigInteger);
        // BigInteger 2개를 서로 합해준다.
        // 출력 결과 : 9223372036854781548
        BigInteger addBigInt = bigInt.add(radixBigInt);

        // BigInteger.subtract(BigInteger);
        // BigInteger 2개를 서로 빼준다.
        // 출력 결과 : 9223372036854770068
        BigInteger subtractBigInt = bigInt.subtract(radixBigInt);

        // BigInteger.multiply(BigInteger);
        // BigInteger 2개를 서로 곱해준다.
        // 출력 결과 : 52942155491546413137920
        BigInteger multiplyBigInt = bigInt.multiply(radixBigInt);

        // BigInteger.divide(BigInteger);
        // BigInteger 2개를 서로 나눠준다.
        // 출력 결과 : 1606859239870170
        BigInteger divideBigInt = bigInt.divide(radixBigInt);

        // BigInteger.remainder(BigInteger);
        // BigInteger 2개를 % 연산 해준다.
        // 출력 결과 : 8
        BigInteger reminderBigInt = bigInt.remainder(radixBigInt);

        // BigInteger.pow(int);
        // BigInteger 를 ^ int 해준다.
        // 출력 결과 : 85070591730234615865843651857942052864
        BigInteger powBigInt = bigInt.pow(2);

        // BigInteger.gcd(BigInteger);
        // BigInteger 2개의 최대 공약수를 BigIntger로 return 해준다.
        // 하나가 0일경우 자기자신, 모두 0일경우 0이 return 된다.
        // 출력 결과 : 4
        BigInteger gcdBigInt = bigInt.gcd(radixBigInt);

        // BigInteger.abs();
        // BigInteger 의 절대값을 return한다.
        // 출력 결과 : 9223372036854775808
        BigInteger absBigInt = bigInt.abs();

        // BigInteger.and(BigIntegr);
        // BigInteger 2개를 & 연산하여 BigIntger로 return한다.
        // 출력 결과 : 0
        BigInteger andBigInt = bigInt.and(radixBigInt);

        // BigInteger.or(BigInteger);
        // BigInteger 2개를 | 연산하여 BigInteger로 return한다.
        // 출력 결과 : 9223372036854781548
        BigInteger orBigInt = bigInt.or(radixBigInt);

        // BigInteger.xor(BigInteger);
        // BigInteger 2개를 ~ 연산하여 BigInteger로 return한다.
        // 출력 결과 : 9223372036854781548
        BigInteger xorBigInt = bigInt.xor(radixBigInt);

        // BigInteger.not();
        // BigInteger를 ~ 연산하여 BigInteger로 return한다.
        // 출력 결과 : -9223372036854775809
        BigInteger notBigInt = bigInt.not();

    }
}

```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[JAVA] #28 BigInteger 클래스 정리[여행벌]](https://travelbeeee.tistory.com/465?category=845655)

[]()

[]()

[]()

[]()

[]()