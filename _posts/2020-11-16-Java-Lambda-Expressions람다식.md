---
title: "[Java] Lambda Expressions(람다식)"
last_modified_at: 2020-11-16T23:37
categories: 
  - java
tags: 
  - 'Java' 
  - 'lambda'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Lambda Expressions?

`Lambda Expressions(람다식)`은 수학자 알론조(Alonzo Church)가 발표한 람다 계산법에서 사용된 식으로, 이를 제자 존 매카시(John Macarthy)가 프로그래밍 언어에 도입했다.

**Java8 버전부터 람다식(Lamdaba Expressions)을 지원**하게 되었다.

**람다식은 익명함수(anonymous function)을 생성하기 위한 식**으로 **식별자없이 실행가능한 함수**이다.
_함수인데 함수를 따로 생성하지 않고 코드 한줄에 함수를 써서 그것을 호출하는 방식_

**객체 지향 언어보다 함수 지향 언어**에 가깝다.
_람다식은 일종의 함수형 프로그래밍에 적합한 문법적 표현방식_
_함수형 프로그래밍은 병렬처리와 이벤트 지향 프로그래밍에 적합_

**람다 형태는 매개변수를 가진 코드 블록**이지만, **런타임 시**에는 **익명 구현 객체(추상메소드를 한개 포함한)를 생성**한다.


**람다식은 문법적으로 간결성으로 기존 Java 문법보다 쉽게 함수를 표현**할 수 있다.

<br>

**람다식은 결국 로컬 익명 구현 객체를 생성하게 되지만, 람다식의 사용 목적은 인터페이스가 가지고 있는 메소드를 간편하게 즉흥적으로 구현해서 사용하는 것이 목적**이다.

---

# Lambda Expressions 특징


**람다 대수는 이름을 가질 필요가 없는 익명 함수 (Anonymous functions)**이다.
_익명함수들은 공통으로 일급객체(First Class citizen)라는 특징을 가지고 있다._
_일급 객체(First Class citizen) :  일반적으로 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체.
함수를 값으로 사용 할 수도 있으며 파라미터로 전달 및 변수에 대입 하기와 같은 연산들이 가능._


**두 개 이상의 입력이 있는 함수는 최종적으로 1개의 입력만 받는 람다 대수로 단순화** 될 수 있다.  
_커링 (Curring)_


---


# Lambda Expressions 장, 단점


장점

1. **코드 간결성** 
람다를 사용하면 **불필요한 반복문의 삭제가 가능하며 복잡한 식을 단순하게 표현**할 수 있다.

2. **코드 가독성**
**코드가 간결**하고 식에 **개발자의 의도가 명확히 드러나므로 가독성이 향상**된다

3. **코드 생산성**
**함수를 만드는 과정없이 한번에 처리할 수 있기에 생산성이 증가**한다.

4. **지연연산 수행 **
람다는** 지연연상을 수행 함으로써 불필요한 연산을 최소화** 할 수 있다.

5. **병렬처리 가능**
**멀티 쓰레드를 활용하여 병렬처리를 사용 할 수 있다.**

단점

1. **람다를 사용하면서 만드는 익명함수는 재사용이 불가능**하다.

2. **람다식 호출과 디버깅이 까다롭다.**

3. **람다 stream 사용의 경우** **단순 for문 혹은 while문 사용 시 성능이 떨어진다.**

3. **불필요하게 많이 사용**하게 되면 오히려 **가독성을 떨어 뜨릴 수 있다.**
**비슷한 함수를 중복 생성할 가능성이 높다.**

4. **재귀로 만들경우에는 다소 부적합**하다.

---

# Lambda Expressions 구조


## Functional Interface

**람다식을 사용하기 위해서는 구현할 Interface가 필요**하다.

**람다식으로 구현하기 위한 인터페이스에는 조건이 하나**있는데** 한개의 추상메소드만 가지고 있어야한다는 것**이다. 

**이러한 인터페이스를 `함수적 인터페이스(Functional Interface)`**라고 부른다.
_**Functional Interface**란 **함수가 하나만 존재하는 Interface를 의미**_


**`Functional Interface`는 함수구현 전용 인터페이스**로, **'구현해야 할 추상 메소드가 하나만 정의된 인터페이스'를 가르킨다.**
_@FunctionalInterface Annotation으로 이런 함수적 인터페이스를 명시 할 수 있다._

**Java Compiler는 함수형 인터페이스에 두 개 이상의 메소드가 선언되면 오류를 발생**시킨다.


<Br>


@FunctionalInterface가 적용된 인터페이스는 한개의 추상메소드만 선언 할 수 있게 된다.

```java
@FunctionalInterface
public interface TestInterface{
    public int plusAandB(int a, int b);
  
//  컴파일 에러 발생   
//  public int minusAandB(int a, int b);

}
```
**@FunctionalInterface가 선언된 인터페이스에 추상메소드가 1개가 아니면 에러가 발생**한다.


  
## Lambda Expressions 문법

람다식의 기본구조는 소괄호에는 구현한 함수의 인자를 그리고 화살표 다음에 중괄호에는 구현할 함수 몸체를 넣어주면 된다.

```
(타입 매개변수, ...) -> { 실행문;...};
```
  
함수를 간편하고 쉽게 표현하기 위해서 람다는 많은 생략 기법을 사용한다.
  
1. 람다는 매개변수 화살표(->) 함수몸체를 이용하여 사용 할 수 있다.
  ```(매개변수) -> {함수몸체}```

2. 매개변수가 하나일 경우 ()를 생략 할 수 있다. 
  ``` 매개변수 -> {함수몸체}```
  
3. 함수몸체가 단일 실행문이면 괄호{}를 생략 할 수 있다.
  ```(매개변수) -> 함수몸체```
  
4. 함수몸체가 return문으로만 구성되어 있는 경우 괄호{}를 생략 할 수 없다.
  ```(매개변수) -> {return 0;}```

5. 람다식 매개변수의 자료형은 생략가능하다.
   
  
```java
//정상적인 유형
() -> {}
() -> 1
() -> { return 1; }

(int x) -> x+1
(x) -> x+1
x -> x+1
(int x) -> { return x+1; }
x -> { return x+1; }

(int x, int y) -> x+y
(x, y) -> x+y
(x, y) -> { return x+y; }

(String lam) -> lam.length()
lam -> lam.length()
(Thread lamT) -> { lamT.start(); }
lamT -> { lamT.start(); }


//잘못된 유형
//선언된 type과 선언되지 않은 type을 같이 사용 할 수 없다.
(x, int y) -> x+y
(x, final y) -> x+y  
```



```java
InterfaceA1 a1 = (int a) -> {System.out.println("a:" + a);};
//매개인자 자료형 생략
InterfaceA1 a2 = (a) - > {System.out.println("a:" + a); };
 
InterfaceB1 b1 = (int a, int b) -> {System.out.println("a+b:" + (a + b));};
//매개인자 자료형 생략
InterfaceB1 b2 = (a, b) -> {System.out.println("a+b:" + (a + b));};
 
//매개인자가 하나뿐이라 소괄호 생략
InterfaceA1 a3 = a -> {System.out.println("a:" + a);};
//함수의 실행문이 한개라 중괄호를 생략
InterfaceA1 a4 = a -> System.out.println("a:" + a);
 
TestInterface t3 = a -> { return "a:" + String.valueOf(a); };
// 함수의 실행문이 한개이며, 리턴문만 있을경우 중괄호와 더불어 return문도 생략이 가능하다.
TestInterface t4 = a -> "a:" + String.valueOf(a);
 
// 매개인자가 없는 경우에는 빈 소괄호를 사용해야 한다.
InterfaceA1 a2 = () -> { System.out.println("인자가 없는 함수 구현"); };
```

---

# Lambda Expressions 예제
  
  
- Functional Interface 선언
  
```java
@FunctionalInterface
interface Gil{
    int log(int a,int b);
}
class Gillog{
    public void logging(Gil gil) {
	int number = gil.log(3, 4);
	System.out.println("logging is "+number);
    }
}
```
- No Using Lambda Expressions
  
```java
Gillog gilLog = new Gillog();
gilLog.logging(new Gil() {
   public int log(int a, int b) {
      System.out.println("This is Gil");
      System.out.println("I'm doing GilLog");
      System.out.println("parameter number is " + a + "," + b);
      return a + b;
      }
});
```
  
  ```
  This is Gil
I'm doing GilLog
parameter number is 3,4
logging is 7
  ```
- Using Lambda Expressions
  
```java
Gillog gilLogLambda = new Gillog();
gilLogLambda.logging((a, b) -> {
   System.out.println("This is Gil Lambda");
   System.out.println("I'm doing GilLog Lambda");
   System.out.println("parameter number is " + a + "," + b);
   return a + b;
});
```
  ```
  This is Gil Lambda
I'm doing GilLog Lambda
parameter number is 3,4
logging is 7
  ```
<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[람다식(Lamdba Expressions) 정리[park_juyoung]](https://juyoung-1008.tistory.com/48)

[[Java] 람다식(Lambda Expressions) -> 사용법 & 예제[코딩팩토리]](https://coding-factory.tistory.com/265)

[[JAVA] 람다식(Rambda)의 개념 및 사용법[사용자 히진]](https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95)

[]()

[]()

[]()
