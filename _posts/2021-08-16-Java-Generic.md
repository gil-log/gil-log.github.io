---
title: "[Java] Generic"
last_modified_at: 2021-08-16T22:46
categories: 
  - java
tags: 
  - 'Generic' 
  - 'Java'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 [JAVA] - 제네릭(Generic)의 이해[st-lab.tistory]](https://st-lab.tistory.com/153)

[Java Generic 자바 제네릭[슬기로운 개발생활 - choi jeong heon]](https://medium.com/%EC%8A%AC%EA%B8%B0%EB%A1%9C%EC%9A%B4-%EA%B0%9C%EB%B0%9C%EC%83%9D%ED%99%9C/java-generic-%EC%9E%90%EB%B0%94-%EC%A0%9C%EB%84%A4%EB%A6%AD-f4343fa222df)

[]()

---

# Generic

**`Generic`**은 **하나의 Data 형식에 의존하지 않고**,

**하나의 값**이 **여러가지 Data Type을 가질 수 있도록 하는 방법**이다.

_`Generic` : 포괄적인, 단 하나에 정해지지않고 범용적이고 일반적인 것_

<br>


**흔히 아래와 같이 사용**된다.


```java
ArrayList<Integer> intList = new ArrayList<Integer>();

ArrayList<String> strList = new ArrayList<String>();

// 객체<타입> 객체명 = new 객체<타입>();
```

위처럼 **`<>` 문구 안에 해당 객체의 Type을 Class 내부가 아닌**,

**외부에서 사용자에 의해 지정되는 것을 의미**한다.


_**`<>`** : **`<>`의 parameter**를 **type paramerters**, **type variable로** 부르고,
**Java7 이후** 부터 **`<>`는 Diamond로 불리운다.** _

<br>

**`Generic`은 한마디로 정의**하면 **특정 Type을 미리 지정하는 것이 아닌**, 

**필요에 의해 지정할 수 있도록 타입을 일반화한 Type**이다.



**이러한 `Generic`은 `Java 5`부터 추가**되었다.

---

## Generic 사용 이유


**`Generic`을 사용하는 이유는 아래 코드**를 살펴보면 알 수 있다.


만약 **아래와 같이 `ArrayList`를 사용하는 경우를 생각**해보자.

```java
// OK
List list = new ArrayList();

// OK
list.add("gillog");

// Type missmatch: cannot convert from Object to String !
String str = list.get(0);
```

**`List`의 Data Type은 `Object`** 이다.

**`Object`는 Java에서 모든 Class의 조상 Class** 이므로 **어떤 종류의 Class로도 접근 가능**하다.

따라서 **2번째 라인까지는 정상 수행 가능**하지만, 

**3번째 라인**에서 **`Object` Type을 return하기 때문**에 **`Type missmatch` Complie Error가 발생**한다.

**해당 3번째 라인이 정상 동작하려면 아래와 같이 수정**되어야 한다.

```java
String str = (String)list.get(0);
```


위와 같은 **`Casting`과정을 거치면 문제없지만**,

**만약 해당 Collection에 수많은 Data**가 들어있고, **모든 Data를 꺼내야 하는 상황**에서는

**매번 `Casting` 과정이 필요**하게 되고, **이는 시스템 성능의 큰 저하를 야기**한다.

<br>

또한 **`String` type의 data가 저장된 Collection**에 **아래와 같이 `Casting`을 시도** 한다면,

```java
// OK
List list = new ArrayList();

// OK
list.add("gillog");

// ClassCastException !
int val = (int)list.get(0);
```

**`ClassCastException`이 발생하며 Application이 종료**될 수 있다.

<br>

**이를 위해 `Java 5` 부터 추가된 것이 바로 `Generic`** 이다.


**다시 위 코드를 `Generic`을 사용**하여보자.


```java

// OK
List<String> list = new ArrayList<>();

// OK
list.add("gillog");

// OK
String str = list.get(0);
```

**`Casting` 과정 없이 Data**를 정상적으로 가져오고, 

**`ClassCastException`에서 자유**로워 질 수 있다.

---


## Generic 사용 장점

**`Generic`을 사용하면 아래의 장점**을 누릴 수 있다.


1. **잘못된 Type(Type Missmatch)을 Complie 단계에서 방지**할 수 있다.

2. **Class 외부에서 Type을 지정함**으로 써, **Type 체크와 변환 과정이 필요 없어 관리하기 용이**하다.

3. **비슷한 기능을 사용할 경우에 코드 재사용성**이 높아진다.



---


## Generic 사용 방법

먼저 **`Generic`의 `Type Parameter Naming Conventions`**를 살펴보자.


|문자|용도|
|:--:|:--:|
|E|Element|
|K|Key|
|N|Number|
|T|Type|
|V|Value|
|S, U, V, ...| 2nd, 3rd, 4th, ... types|

**`Generic`은 반드시 한 글자일 필요는 없지만**, **Oracle에서 권장**하는 **사용 용도별 Generic 문자 목록**이다.

그럼 **해당 문구를 가지고 Generic을 사용하는 방법**을 **아래 코드와 함께** 살펴보자.


<br>


### Class 및 Interface

```java
public class Gillog <T> {}
```

**`Generic Type`은 기본적**으로 **Class나 Interface에서 위와 같은 형태로 선언**한다.


**두 개의 `Generic Type`을 작성하는 것 역시 가능**하다.

```java
public Interface Gillog <T, K> {}
```

이렇게 **해당 Class나 Interface에서 사용할 타입**을 **Class, Interface 외부에서 지정할 수 있게 사용**한다.

<br>

**`Generice Type`을 활용한 Class나 Interface를 사용할 때**는,

**해당 객체 생성 시점에 구체적인 타입을 명시**해야 한다.


```java

public Class Gillog <T, K> {

        public static void main(String[] args) {
            Gillog<String, Integer> gillog = new Gillog<String, Integer>();
        }

}
```

여기서 `T`는 `String` Type이 되고, `K`는 `Integer` Type이 된다.

이때, **`Type Parameter`로 명시 가능한 Type은 `Reference Type(참조 타입)`만이 가능**하다.

**int, double, boolean, char 와 같은 `primitive type(원시 타입)`은 지정할 수 없다.**


<br>


만약 **`Type Parameter`의 Type을 제한하고 싶은 경우** 아래와 같이 **`extends`를 활용**하면 된다.


```java
public Class Gillog <T, K> {

        public <U extends Number> void logNumber(U u) {
            System.out.println("Log ==" + String.valueOf(u));
        }

        public static void main(String[] args) {
            Gillog<String, Integer> gillog = new Gillog<String, Integer>();
                gillog.logNumber(new Integer(286));
                // error excuted
                gillog.logNumber("286");
        }

}
```

### Generic Method


**`Generic`을 Method에 선언하여 사용 역시 가능**하다.

```java
public <T> gillogMethod(T t) { ... }

AccessModifier <GenericType> ReturnType MethodName(GenericType parameterName)
```

**Generic Class와 함께 사용해보면 아래**와 같다.


```java
public Class Gillog <T> {

        public <U> void logNumber(U u) {
            System.out.println("Log ==" + String.valueOf(u));
        }

        public static void main(String[] args) {
            Gillog<Integer> gillog = new Gillog<Integer>();
            gillog.logNumber(new Integer(286));
            // error excuted
            gillog.logNumber("286");
        }

}
```


이렇게 **Generic Class**와 **Method**에서 **별도의 Generic을 구분지어 사용하는 이유**는,

**Generic Method를 static method로 사용할 때 필요**하기 때문이다.


만약 아래 코드를 살펴 보면

```java
public Class Gillog <T extends Number> {

	public void logNumber(T t) {
            System.out.println("Log ==" + String.valueOf(t));
        }
        
        public static <U extends Number> void staticLogNumber(U u) {
            System.out.println("Log ==" + String.valueOf(u));
        }

        public static void main(String[] args) {
            Gillog<Integer> gillog = new Gillog<Integer>();
            gillog.logNumber(new Integer(286));
            
            Gillog.staticLogNumber(new Integer(286));
        }

}
```

**`logNumber()` method에서 사용할 Generic Type `T`**의 경우, 

**유형 지정을 위해**서 **new 생성자로 `Gillog <T extends Number>` Class**를,

**Instance화 시킬 때** `Integer` **Type을 지정**해주었다.



**하지만 static method** 인 **`staticLogNumber()`의 경우** **이미 JVM 구동시에 Memory에 onload 되는 형태**이다.

따라서 **해당 Class를 Instance화 시키는 과정이 없어 Type 지정을 생성자를 통해 지정할 수 없다.**

**이럴때 별도의 Generic을 사용**해야 한다.



---

## Type Parameter 제한

**Gneric을 사용**할 때 **`Type Parameter`의 범위를 제한 할 수 있다고 앞서 설명**했다.

**`Type Parameter`의  범위를 제한할 때**는 **`extends`, `super`를 사용할 수** 있는데

**간단히 먼저 설명**하면

**`extends`**는 **해당 Class, sub Class 포함 Class로 범위를 제한**하고,


**`super`**는 **해당 Class, super Class 포함 관계 Class로 범위를 제한**한다.

![](https://images.velog.io/images/gillog/post/9881478b-6ab0-4151-9325-32b687086591/image.png)

**위와 같은 관계의 Class**가 있을때 **`extends`와 `super`를 활용한 Type 지정은 아래**와 같다.


### extends
**`extends` 는 해당 타입을 포함한 자식 타입 까지만 가능**하다.

```
// L, I Type 가능
<K extends I> 

// L Type만 가능
<K extends L> 

// O Type만 가능
<K extends O> 

// G, I, L, O Type 가능 
<K extends G> 

```

### super
**`super`는 해당 타입을 포함한 부모 타입만 가능**하다.


```
// I, G Type 가능
<K super I> 

// L, I, G Type만 가능
<K super L> 

// O, G Type만 가능
<K super O> 

// G Type만 가능 
<K super G> 
```
