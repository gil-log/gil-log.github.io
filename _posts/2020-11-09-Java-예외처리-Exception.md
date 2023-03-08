---
title: "[Java] 예외처리 (Exception)"
last_modified_at: 2020-11-09T11:15
categories: 
  - java
tags: 
  - 'Java' 
  - 'exception'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 예외처리(Exception)

**`에러(Error)`란** 컴퓨터 **하드웨어의 오동작 또는 고장**으로 인해 **응용프로그램에 이상**이 생겼거나 **JVM 실행에 문제**가 생겼을 경우 발생 하는 것을 말하며, 이 경우 **개발자가 대처할 방법이 극히 제한적이다. **

**`예외(Exception)`란 사용자의 잘못된 조작** 또는 **잘못된 코딩으로 인해 발생하는 프로그램 오류**를 말한다. 

**`예외`가 발생하면 프로그램이 종료가 된다는것은 에러와 동일**하지만 **`예외`는 `예외처리(Exception Handling)`을 통해 프로그램을 종료 되지 않고 정상적으로 작동되게 만들어줄 수 있다. **

**Java에서 `예외처리`는 Try Catch문을 통해 해줄 수 있다.**


### 예외 구문들

|예외 구문|이유|
|:--:|:--:|
|ArithmeticException|정수를 0으로 나눌경우 발생|
|ArrayIndexOutOfBoundsExcetion|배열의 범위를 벗어난 index를 접근할 시 발생|
|ClassCastExcetion|변환할 수 없는 타입으로 객체를 반환 시 발생|
|NullPointException|존재하지 않는 레퍼런스를 참조할때 발생|
|IllegalArgumentException|잘못된 인자를 전달 할 때 발생|
|IOException|입출력 동작 실패 또는 인터럽트 시 발생|
|OutOfMemoryException|메모리가 부족한 경우 발생|
|NumberFormatException|문자열이 나타내는 숫자와 일치하지 않는 타입의 숫자로 변환시 발생|


### 빈도 높은 예외

- **NullPointException**

Java에서 가장 빈번하게 발생하는 예외. 객체 참조가 없는 상태, null 값을 갖는 참조변수로 객체에 접근을 시도할 때 발생한다.
_**객체가 없는 상태에서 객체를 사용하려 했으니 예외가 발생하는 것**_



- **ArrayIndexOutOfBoundsExcetion**

**Array에서 Index 범위를 초과하여 사용할 경우 발생**한다.
길이가 4인 `int[] arr = new int[4]` 배열을 선언했다면** 0 ~ 3까지의 index만 사용**할 수 있다.
하지만 이 **배열의 index가 -1 이나 4을 참조하는 순간 예외가 발생**한다.


- **NumberFormatException **

개발을 하다보면 문자열로 되어있는 데이터를 숫자타입으로 변경하는 경우가 자주 발생하는데 **숫자타입으로 변경할 수 없는 문자를 치환시키려고 하면 발생하는 예외**이다.



- **ClassCastExcetion**

**타입 변환은 상위 Class와 하위 Class간에 발생**하고 **구현 Class와 Interface간에도 발생**한다.

**이런 관계가 아니면 Class는 다른 Class로 타입을 변환할 수 없다.**

하지만 **이 규칙을 무시하고 억지로 타입을 변환 시킬경우 발생하는 예외**이다.






# throw와 throws




### throw

**`throw`는 강제로 예외를 발생시키는 경우에 사용**한다.
**throw 예약어 뒤에는 `java.lang.Throwable Class`를 상속받은 자식 Class의 객체를 지정**해야 한다.



```java
class TestException {
  public static void main(String[] args) {
    try {
      throw new Exception();
    } 
    catch (Exception e) {
      System.out.println(e);
    }
  }
}
```


**`throw`는 메소드내에서 상위 블럭으로 예외를 던지는 것**이다.

**`throw`는 고의적으로 에러를 발생시킬때도 사용**되지만 **현재 메소드의 에러를 처리한 후**에,

**상위 메소드에 에러 정보를 줌으로써 상위 메소드에서도 에러가 발생 감지용으로도 사용**된다.

<br>


### throws

**`throws`**는 **메소드를 정의할 때 `throws` 예약어를 시그내처에 추가**하면 그 **메소드를 호출하는 곳에서 예외 처리를 해야 한다.**

즉, **현재 메소드에서 상위 메소드로 예외를 전가**시키며 **자신이 예외를 처리하지 않고**, 자신을 **호출한 곳에서 예외처리의 책임을 전가하는 것**이다.

- **[Ex] aMethod throws SomeException**

**aMethod이 SomeException 예외를 던진다는 뜻**이므로 **aMethod을 호출(사용)하는 블록을 try 블록으로 감싸준다.**

```java
class TestException {
  public static void aMethod() throws ArithmeticException {
    int a = 0;
    a = 10/a;
  }

  public static void main(String[] args) {
    try {
      // 이 함수가 예외를 던진다. 
      // 주변을 try 블록으로 감싸준다.
      TestException.aMethod(); 
    } catch (Exception e) {
      System.out.println("main() 메소드가 예외를 잡아서 처리한다: " + e);
    }
  }
}
```







# 예외 처리 코드

```java
try{
    //에러가 발생할 수 있는 코드
    throw new Exception(); //강제 에러 출력 
}catch (Exception e){
    //에러시 수행
     e.printStackTrace(); //오류 출력(방법은 여러가지)
     throw e; //최상위 클래스가 아니라면 무조건 던져주자
}finally{
    //무조건 수행
} 


```

![](https://images.velog.io/images/gillog/post/1e423f36-0b26-4c8c-8c06-64d9d079a849/img1.daumcdn.jpg)


**try 블록에는 예외가 발생할 수 있는 코드가 위치**한다. 

**try 블록의 코드에서 예외가 발생하면 즉시 실행을 멈추고 catch 블록으로 이동하여 예외처리 코드를 실행**하고  finally 블록의 코드를 실행한다.

**try 블록의 코드가 정상 실행되면 catch블록의 코드는 실행되지 않고 finally 블록의 코드를 실행**한다.


try catch 문은 주로 **데이터베이스에 데이터를 주고받을 경우에 많이 사용**한다. 
_데이터베이스를 거쳐올 때는 변수가 많이 생기기 때문에 필수적_ 

**그 후 finally에는 데이터베이스와의 연결을 끊어주는 코드를 주로 삽입**한다.
_특정 예외가 발생하여 데이터베이스와의 연결이 끊어지지 않으면 여러가지 문제를 야기할 수 있기 때문_


**최상단 클래스를 제외한 나머지 클래스에서의 예외처리는 반드시 Throw**를 해주어야 한다. 

그렇지 않으면 **예외처리를 해주었음에도 불구하고 Main에서는 Exception을 전달받지 못하여 예외를 인지 못하는 경우가 발생**한다.

![](https://images.velog.io/images/gillog/post/a4cc3fc5-9829-4be9-b624-d64f7ecc98fb/img1.daumcdn.png)









<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[Java] 자바 예외처리 Try Catch문 사용법[코딩팩토리]](https://coding-factory.tistory.com/280)

[ 점프 투 자바 07장 자바 날개 달기 07-4 예외처리 (Exception)](https://wikidocs.net/229)


[[JAVA] throw와 throws 의 차이점[Developer Vitalholic]](https://vitalholic.tistory.com/246)

[Spring Guide - Exception 전략 - Posted by Yun](https://cheese10yun.github.io/spring-guide-exception/#business-exception)

[]()