---
title: "String Class와 성능 이슈, String 제대로 사용하기"
last_modified_at: 2021-05-04T00:12
categories: 
  - 자바성능튜닝이야기
tags: 
  - 'Java' 
  - 'StringBuffer' 
  - 'StringBuilder' 
  - 'string class' 
  - '자바 성능 튜닝 이야기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>


# String Class와 성능

**`String` Class는 모든 개발자들이 알면서도 잘 지키지 않는 Class중 하나**이다.

바로 **성능과 밀접하게 영향을 끼칠 수 있다는 사실을 알면서도, 그것에 대해 깊게 생각하지 않는 것**인데, **`String Class`는 잘 사용하면 상관 없지만 잘못 사용하면 메모리에 많은 영향**을 준다.

<br>

먼저 **String이 GC에 영향을 주는것은 확실한 사실**이다.

하지만 **이것만 고친다고 개발한 Application의 메모리가 효율적으로 사용된다는 것을 의미하지는 않는다.**
_사실 **대부분의 성능 누수는 라이브러리나 시스템 티어를 추가시킬때 마다 몇 배씩 증가**한다._

**성능 개선에 있어서 작은 부분**이지만, **이것부터 적용을 시작하자는 의미**이고, **성능 개선을 위한 첫 단추로 삼자는 것**이다.

<br>

# String Class의 잘못된 사용

**Java 기반 프로그래밍에서 `java.lang.Object`를 제외 하고 가장 많이 사용하는 객체**는 **int, long 등의 기본 자료형(Primitive Type)을 제외**하면 **1위로 `String Class`, 2위로 `Collcetion 관련 Class`**를 들 수 있다.


<br>

대부분의 **`Web 기반 System`은 DB에서 Data를 가지고와 이를 화면에 출력하는 System**이기에, **`쿼리 문장`을 만들기 위한 `String Class`**와 **`결과 처리`를 위한 `Collection Class`를 가장 많이 사용**하게 된다.


대표적인 예를 들어 **Java에서 쿼리문을 생성 할때 String Type으로 된 문자열들을 더해가는 형태로 쿼리문을 만드는 경우**가 있다.
_이러한 경우 **GC의 발생 빈도 등을 통해서도 성능 테스트에서 확인 가능**_

아래는 예시 코드 이다.

```java
String strSQL = "";
strSQL += "select * ";
strSQL += "from GILLOG";
```





**이제는 `myBatis`, `Hibernate`와 같은 `Data Mapping Framework`를 사용**하지만, **예전 System에서는 이렇게 쿼리를 작성**했다.

단일 쿼리 문장이 아니더라도, **문자열을 다루는 경우에 이런식으로 코드를 작성**하면 **메모리를 많이 사용하게 된다는 문제가 발생**한다.


위 **코드 라인을 같은 패턴으로 100회 수행한다는 가정**하에 **테스트를 돌려보면 아래와 같은 결과**를 얻을 수 있다.

|구분|결과|
|:--:|:--:|
|메모리 사용량|10회 평균 약 5MB|
|응답 시간|10회 평균 약 5ms|


**이럴 때는 StringBuffer나 StringBuilder Class를 이용하면 메모리 사용량을 줄일 수** 있다.

성능 개선 코드는 아래와 같다.

```java
StringBuilder strSQL = new StringBuilder();
strSQL.append(" select * ");
strSQL.append(" from GILLOG ");
```

**위 코드를 같은 패턴으로 동일한 테스트를 수행하면 얻을 수 있는 결과는 아래**와 같다.

|구분|결과|
|:--:|:--:|
|메모리 사용량|10회 평균 약 371KB|
|응답 시간|10회 평균 약 0.3ms|


**이렇게까지 성능 차이가 날 수 있는 원인**은 무엇인지는

**`StringBuilder Class`를 살펴보면** 알 수 있다.

---


# StringBuffer Class, String Builder Class

**`JDK 5.0` 기준 문자열 생성 Class는 `String`, `StringBuffer`, `StringBuilder`가 가장 많이 사용**된다.


여기서 **`StringBuilder` Class는 JDK 5.0에서 새로 추가** 되었다.

**`StringBuffer`나 `StringBuilder`에서 제공하는 Methods는 동일**하다.

그렇다면 **`StringBuffer`, `StringBuilder` 두 Class의 차이는 무엇**일까?

<br>

## Buffer vs Builder = ThreadSafe

**`StringBuffer` Class는 `Thread Safe`하게 설계**되어 있어, **여러개의 Thread에서 하나의 `StringBuffer` 객체를 처리해도 전혀 문제가 되지 않는다.**

**`StringBuilder`는 단일 Thread에서 안전성만을 보장**하기에 **여러개의 Thread에서 `StringBuilder` 객체를 처리하면 문제가 발생**한다.

<br>

## Constructor, Methods

**두 Class의 Constructor와 Methods를 확인하고 정리**해보면 아래와 같다.
_**StringBuffer와 StringBuilder는 동일하게 제공**하여 **Buffer를 기준으로 작성**_

|생성자|설명|
|:--:|:--:|
|StringBuffer()|아무 값도 없는 StringBuffer 객체를 생성한다.<br>기본 용량은 16개의 `char`|
|StringBuffer(CharSequence seq)|CharSequence를 매개변수로 받아 그 seq 값을 갖는 StringBuffer를 생성한다.|
|StringBuffer(int capacity)|capacity에 지정한 만큼 용량을 갖는 StringBuffer를 생성한다.|
|StringBuffer(String str)|str 값을 가지는 StringBuffer를 생성한다.|


<br>

### CharSequence ????

**`CharSequence`는 Interface**이다.

**Class가 아니기에 Instance화 시킬 수 없고, 이 Interface를 구현한 Class**로는 **`CharBuffer`, `String`, `StringBuffer`, `StringBuilder`**가 있다.

**주로 `StringBuffer`, `StringBuilder`로 생성한 객체를 전달하는 용도로 사용**된다.

아래 코드는 **`CharSequence`를 활용하여 `StringBuffer`객체를 처리하는 코드**이다.

```java
public class StringBufferTest {
    public static void main(String args[]) {
        StringBuilder sb = new StringBuilder();
        sb.append("Gillog");
        StringBufferTest stringBufferTest = new StringBufferTest();
        stringBufferTest.check(sb);
    }
    public void check(CharSequence cs) {
        StringBuffer sb = new StringBuffer(cs);
        System.out.println("sb.length=="+sb.length());
    }
}
```
위 코드를 수행하면 `sb.length=6`이라는 결과가 처리된다.

즉, **`StringBuffer`나 `StringBuilder`로 값을 만든 후 굳이 toString을 수행**하여 **필요 없는 객체를 만들어서 넘겨주기보다**는 **`CharSequence`로 받아서 처리하는것이 메모리 효율에 더좋다.**



### append(), insert()

**`StringBuffer`와 `StringBuilder`**에서 **자주 사용하는 Methods는 `append()`와 `insert()` Method**가 있다.

이 **두 Method는 여러 가지 타입 매개변수를 수용**하며, **아래와 같은 타입들을 매개변수로 사용**할 수 있다.

```java
boolean
char
char[]
CharSequence
double
float
int
long
Object
String
StringBuffer
```

**`append()`는 말 그대로 기존 값 맨 끝자리에 넘어온 값을 덧붙이는 작업을 수행**한다.

**`insert()`는 지정된 위치 이후에 넘어온 값을 덧붙이는 작업을 수행**한다.
_**지정한 위치까지 값이 할당되어 있지 않으면 `StringIndexOutOfBoundsException`이 발생**_

사용 예제는 아래와 같다.


```java
public class StringBufferTestTwo {
    public static void main (String args[]) {
        StringBuffer sb = new StringBuffer();

        // 기본 사용
        sb.append("GIL");
        sb.append("LOG");
        sb.append("GILLOG");
        
        // 이렇게도 가능
        sb.append("this")
        .append("way")
        .append("can")
        .append("work");
        
        // insert 사용
        sb.insert(1,"|||");

        // 이렇게는 사용하면 StringBuffer 사용 효과 전혀 없음
        sb.append("ButDo"+"Not"+"ThisWay");
        System.out.println(sb);
    }
}
```
위 예제에서 **`append()`안에 `+`를 이용해 문자열을 덧붙이면 `StringBuffer`를 사용하는 효과가 전혀 없으니, `append()`를 이용해 문자열을 덧붙여야 한다.**

**왜 `+`를 이용하면 효과가 없는지는 아래에서 설명**한다.

## String VS StringBuffer VS StringBuilder


**`Profiling Tool`을 사용하여 `String`, `StringBuffer`, `StringBuilder`를 비교해 볼 것**이다.

**이를 확인**하고 나면 **왜 `append()`를 이용해 문자열을 더해야 하는지** 알 수 있다.


```java
<%
final String valueStr = "gillog";
for(int outLoop = 0; outLoop < 10; outLoop++) {
	String str = new String();
    StringBuffer buffer = new StringBuffer();
    StringBuilder builder = new StringBuilder();
    
    for(int loop = 0; loop < 10000; loop++) {
    	str += valueStr;
    }
    
    for(int loop = 0; loop < 10000; loop++) {
    	buffer.append(valueStr);
    }
    String tempBuffer = buffer.toString();
    
    for(int loop = 0; loop < 10000; loop++) {
    	builder.append(valueStr);
    }
    String tempBuilder = builder.toString();
}
%>
OK
<%= System.currentTimeMillis() %>
```

해당 **테스트를 JSP로 만든 이유는 java 파일로 만들어 반복문 작업을 수행하면 `Class`를 `Memory`에 `Loading` 하는데 소요되는 시간이 발생 하기 때문**이다.

_**Java 기반 Application의 응답을 측정하는 경우, 처음 수행하는 화면의 결과 값은 무조건 무시**해야 한다.
**`Class`를 `Memory`로 `Loading`할때 시간이 오래 걸리기 때문에, 그때 측정 결과 값은 의미가 없기 때문**이다._

<br>

그래서 **JSP로 테스트를 만들고 최초 화면 호출 응답 시간과 메모리 사용량은 측정에서 제외**하고, **두 번째 호출 내용부터 최상단의 for문 `outLoop`에서 10회 반복 수행한 결과의 누적 값을 구한 결과**는 아래 표와 같다.



|소스 부분|응답 시간(ms)|응답 시간(sec)|
|:--:|:--:|:--:|
|`str += valueStr;`|95,801.41|95
|`buffer.append(valueStr);`<br>`String tempBuffer = buffer.toString();`|247.48<br>14.21|0.24|
|`builder.append(valueStr);`<br>`String tempBuilder = builder.toString();`|174.17<br>13.38|0.17|

<br>

|소스 부분|메모리 사용량(bytes)|생성된 임시 객체 수|비고|
|:--:|:--:|:--:|:--:|
|`str += valueStr;`|100,102,000,000|4,000,000|약 95GB
|`buffer.append(valueStr);`<br>`String tempBuffer = buffer.toString();`|29,493,600<br>10,004,000|1,200<br>200|약 28MB<br>약 9.5MB|
|`builder.append(valueStr);`<br>`String tempBuilder = builder.toString();`|29,493,600<br>10,004,000|1,200<br>200|약 28MB<br>약 9.5MB|

**응답 시간 부분**에서 **`String`보다 `StringBuffer`가 367배, `StringBuilder`가 512배 빠르다.**

**메모리 부분**에서 **`String`가 `StringBuffer`와 `StringBuilder`보다 3,390 배 더 사용**한다.


**이러한 결과가 발생하는 원인은 `String`의 수행 구조 때문**이다.

### String Class 원리

```java
final String valueStr = "gillog";
String str = new String();
for(int loop = 0; loop < 10000; loop++) {
    str += valueStr;
}
```
위 `str += valueStr;`의 3회 수행을 나타내면 아래와 같다.

|||||
|:--:|:--:|:--:|:--:|
|주소=100|gillog|||
|주소=150|gillog|gillog||
|주소=200|gillog|gillog|gillog|

<br>

제일 먼저 **`str` 객체에는 `gillog`값이 저장**되어 있는 상태로 **`100 주소`으로 `Heap Area`에 load**된다.

`str += valueStr` **코드가 수행되면 `100 주소`, `str` 객체는 `Garbage`가 되고, `150 주소`, `str` 객체가 새로 생성**된다.

`str += valueStr` 코드가 **세 번째 수행되면, `150 주소`, `str` 객체는 `Garbage`가 되고, `200 주소`, `str` 객체가 새로 생성**된다.

이런 작업이 **반복 수행되면 메모리를 많이 할당하여 사용하고, 응답 속도에 많은 영향을 끼친다.**

### String Buffer, Builder 원리

`String Buffer`나 `StringBuilder`는 **`String`과 다르게 새로운 객체를 생성하지 않고, `Heap Area`**에 **이미 존재하는 객체의 크기를 증가시키면서 값을 더해간다.**

|||||
|:--:|:--:|:--:|:--:|
|주소=100|gillog|||
|주소=100|gillog|gillog||
|주소=100|gillog|gillog|gillog|

<br>

그렇다고 해서, `String`을 사용하는것이 무조건 나쁜것이고, **`String Buffer`나 `String Builder`를 반드시 사용해야 하는것은 아니다.**

아래와 같은 **상황에서 잘 구분하여 사용하면 바람직**하다.


1. `String`은 **짧은 문자열을 더할 경우 사용**

2. **`StringBuffer`는 `Thread Safe` 프로그램이 필요** 할때, **개발 중 시스템 부분이 `Thread Safe` 인지 모를 경우 사용**하면 좋다.

3. **`Class`에 `static`으로 선언한 문자열을 변경, `singleton`으로 선언된 `Class`에 선언된 문자열일 경우 `StringBuffer`를 반드시 사용**해야 한다.

4. **`StringBuilder`**는 **`Thread Safty` 상관 없이 전혀 관계 없는 프로그램을 개발할때, method 내 변수 선언**은 그 **method안에 지역 변수로 존재하므로 `StringBuilder`를 사용**하면 된다.



## JDK 1.5 up???

사실 **지금 까지 내용들은 `JDK 1.5` 이상의 버전을 사용하고 있다면, `Compiler`에서 자동으로 `StringBuilder`로 변환**해준다.


아래의 코드를 살펴 보면

```java
public class Gillog {
	String gil = "Im " + "Gil" + "log.";
    public Gillog() {
    	int version = 1;
        String log = "Im " + "Gil" + version + "log.";
    }
}
```

위 코드를 **`JDK 1.4`에서 `JAD`를 사용하여 역 컴파일한 소스는 아래**와 같다.

```java
public class Gillog {
    public Gillog() {
    	gil = "Im Gillog.";
    	int version = 1;
        String log = "Im Gil" + version + "log.";
    }
    String gil;
}
```

**`JDK 1.4` 버전에서 역 컴파일한 소스를 살펴보면, `Java Compiler`**가 문자열 더한 것을 **`Compile`할 때 알아서 더하는 과정에서 중간에 `int`같은 다른 타입이 더해지면** **`Im Gil`, `log.`과 같이 `Garbage` 객체가 생성**된다.


<br>

`JDK 1.5`에서 달라진 과정을 살펴보면 아래와 같다.


```java
public class Gillog {
    public Gillog() {
    	gil = "Im Gillog.";
    	int version = 1;
        String log = (new StringBuilder("Im Gil"))
        .append(version)
        .append("log.")
        .toString();
    }
    String gil;
}
```

문자열을 그냥 더하는 코드가 **`JDK 1.5`이상 버전에서는 `Compile` 될때 자동으로 `Compiler`가 `StringBuilder`로 변환**해준다.


# So What?

문자열 처리 `String`, `StringBuffer`, `StringBuilder` 세 가지 Class 중 **메모리, 응답 시간에 가장 많은 영향을 주는 `String`**은 `WAS`나 `System`의 **`JDK 1.5`이상 버전을 사용한다면 `Compiler`가 자동으로 `StringBuilder`로 변환**해 준다.

하지만 반복 루프 문에서는 **객체를 `Garbage`를 늘려가며 추가 한다는 사실에 변함 없다.**

그러므로 **`String`대신 `Thread`와 관련 있다면 `StringBuffer`, `Thread`와 관련 없다면 `StringBuilder`를 사용하는 것을 권장**한다.