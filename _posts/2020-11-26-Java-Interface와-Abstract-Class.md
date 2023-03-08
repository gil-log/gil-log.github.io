---
title: "[Java] Interface"
last_modified_at: 2020-11-26T23:41
categories: 
  - java
tags: 
  - 'Java' 
  - 'interface'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Interface

**`Interface`**는 **동일한 목적 하에 동일한 기능을 수행하게끔 강제하는 것**이 바로 **`Interface`의 역할이자 개념**이다. 

**`Interface`는 `interface` 키워드를 통해 선언**할 수 있으며 **`implements` 키워드를 통해 일반 Class에서 Interface를 구현**할 수 있다.

`Interface`는 **Java의 다형성을 극대화**하여 **개발코드 수정을 줄이고 프로그램 유지보수성을 높이기 위해 사용**한다.

## Java8과 Interface

**JAVA8 이전까지는 `Interface`에서 constant(상수), abstract method(추상 메소드)만 선언이 가능**하지만,
_상수, 추상메소드만 가능케했다는 것을 통해 그만큼 강제성이 강했다는 것을 유추할 수 있다._


**JAVA8부터 default method, static method도 선언할 수 있게 추가**되었다.
_default method, static method를 통해, 구현 강제성 안에 유연함을 심었다._

### default method, static method??

**`Interface`에서 `default` 키워드로 method가 선언되면 해당 method가 구현될 수 있다. **

**또한 이를 구현하는 Class**는 **`default method`를 그대로 사용할 수도, Overriding 할 수도 있다.**

이를 통해 **Interface가 변경이 되면, Interface를 구현하는 모든 Class들이 해당 method를 구현해야 하는 문제**를 **`default method`를 통해 Interface에 method를 구현함으로써 해결** 하였다.

또한 **`Interface`에 `static method`를 선언함으로써**, **Interface를 이용하여 간단한 기능을 가지는 유틸리티성 Interface를 만들 수 있게 되었다.**

### default method vs static method

#### default method

- **interface에서도 메소드 구현이 가능**하다.

- **참조 변수로 함수를 호출**할 수 있다.
_EX : `gil.defaultmethod();`_

- **implements한 Class에서 재정의가 가능**하다.

#### static method 

- **interface에서 메소드 구현이 가능**하다.

- **반드시 클래스 명으로 메소드를 호출**해야 한다.
_EX : `Gillog.staticmethod();`_

- **재정의가 불가능**하다.




<br>

따라서 Java8 이후 부터는 Interface에서 아래와 같이 4가지 요소를 정의하거나 구현할 수 있다.

```java
public interface GillogInterface {

    // constant
    // initialized 되야 한다.
    // 자동으로 public static final이 붙는다.
    int level = 1;

    // abstract method
    // abstract는 생략 가능하다.
    // body를 통해 구현할 수 없다.
    public abstract int develop(boolean choice);

    // default method
    // body를 통해 구현되야 한다.
    // Overriding 가능.
    default void gillog(Date date) {
        System.out.println(date + " 날짜의 글 생성");
    }

    // static method
    // body를 통해 구현되야 한다.
    // 재정의 불가능.
    static int levelup(int level) {
        int levelup = level + 1;
        return levelup;
    }

}
```

이때 Interface에서 정의하거나 구현한 4가지 요소들의 의미는 아래와 같다.


constant(상수) :** Interface에서 상수 값을 정해줄테니 함부로 바꾸지 말고 제공해주는 값만 참조**해야 한다.
_절대적_


abstract method(추상 메소드) : **메소드에 대한 기본 가이드만 제공**하니 **추상 메소드를 Overriding해서 구현해야 한다.**
_강제적_


default method(디폴트 메소드) :** Interface에서 기본적으로 제공**해주지만,** custom해서 각자 구현해서 사용할 수도 있다.**
_선택적_


static method(정적 메소드) : **Interface에서 제공해주는 것으로 무조건 사용**해야 한다.
_절대적_


# Constant Interface

`Constant Interface`란 **오직 Constant(상수)만 정의한 Interface**이다.

**Interface의 경우, 변수를 선언할 때 자동으로 `public static final`이 붙는다. **
_상수처럼 어디에서나 접근할 수 있다. _

하나의 Class에서 여러 개의 Interface를 Implement 할 수 있는데, **`Constant Interface`를 Implement 할 경우**, **Interface의 Class 명을 네임스페이스로 붙이지 않고 바로 사용**할 수 있다. 

**아래와 같은 편리성 때문에 Constant Interface를 사용**한다. 

```java
public interface GilLevelConstants {
    double LEVEL = 1;
    double GROWPOINT = 1.01;
}

public class GilLevelClassImpl implements GilLevelConstants {
    public double growUp() {
        return LEVEL * GROWPOINT;
    }
}
```


## But Anti Pattern

Constant Interface를 써도 컴파일이 안 되는 것도 아니고 그렇게 잘못된 것 같지는 않아 보인다. 하지만 위키와 Effective Java (규칙19) 책을 보면 다음과 같은 이유로 Anti 패턴으로 간주한다.



1. **사용하지 않을 수도 있는 상수를 포함하여 모두 가져오기 때문에 계속 가지고 있어야 한다.**



2. **컴파일할 때 사용되겠지만, 런타임에는 사용할 용도가 없다.**
_Marker Interface는 런타임에 사용할 목적이 있으므로 다름_



3. ** Binary Code Compatibility(이진 호환성)을 필요로 하는 프로그램일 경우**, 새로운 라이브러리를 연결하더라도, **상수 인터페이스는 프로그램이 종료되기 전까지 이진 호환성을 보장하기 위해 계속 유지되어야 한다.**



4. ** IDE가 없으면**, **상수 인터페이스를 Implement 한 클래스에서**는 **상수를 사용할 때 네임스페이스를 사용하지 않으므로**,** 해당 상수의 출처를 쉽게 알 수 없다.** 
**또한 상수 인터페이스를 구현한 클래스의 하위 클래스들의 네임스페이스도 인터페이스의 상수들로 오염**된다.



5. **인터페이스를 구현해 클래스를 만든다는 것**은, **해당 클래스의 객체로 어떤 일을 할 수 있는지 클라이언트에게 알리는 행위**이다. 
**따라서 상수 인터페이스를 구현한다는 사실은 클라이언트에게는 중요한 정보가 아니다.**
다만, **클라이언트들을 혼동시킬 뿐**이다.



6. **상수 인터페이스를 Implement 한 클래스에 같은 상수를 가질 경우**, **클래스에 정의한 상수가 사용**되므로** 사용자가 의도한 흐름으로 프로그램이 돌아가지 않을 수 있다. **
아래는 간단한 예제이다.


```java
public interface GilLevelConstants { 
    public static final int LEVEL = 1; 
} 

public class GilLevel implements GilLevelConstants { 
    public static final int LEVEL = 2; 
    public static void main(String args[]) throws Exception {
        // 2가 출력.
        System.out.println(LEVEL); 
    } 
}
```

## So use import static

**자바 공식 문서에서도 Constant Interface를 Anti 패턴으로 명시**하였고 **이 대체 방안으로 `import static`구문 사용을 권장**한다.
**Constant Interface와 동일한 기능과 편리성을 제공**한다. 

아래는 간단한 예제이다.

```java
public final class GilLevelFinalConstants {
    // 인스턴스화 방지
    private GilLevelFinalConstants() {
    }

    public static final double LEVEL = 1;
    public static final double GROWPOINT = 1.01;
}

import static GilLevelFinalConstants.LEVEL; 
import static GilLevelFinalConstants.GROWPOINT; 

public class GillogLevelClass { 
    public double growUp() {
        return LEVEL * GROWPOINT;
    } 
}
```


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[JAVA] 자바 인터페이스란?(Interface)_이 글 하나로 박살내자[Limky 삽질블로그]](https://limkydev.tistory.com/197?category=957527)

[인터페이스의 default method[Programmers]](https://programmers.co.kr/learn/courses/5/lessons/241)

[[JAVA] interface (default, static메소드)[dahye_e]](https://dahyeee.tistory.com/entry/JAVA-interface-default-static%EB%A9%94%EC%86%8C%EB%93%9C)

[Constant Interface : 상수를 인터페이스에?](https://m.blog.naver.com/PostView.nhn?blogId=01032707009&logNo=221055902106&categoryNo=48&proxyReferer=&proxyReferer=https:%2F%2Fwww.google.com%2F)

[자바의 추상 클래스와 인터페이스 - 추상 클래스와 인터페이스의 차이[by강관우]](https://brunch.co.kr/@kd4/6)

[[JAVA] 추상클래스 VS 인터페이스 왜 사용할까? 차이점, 예제로 확인 [마이자몽 myJamong]](https://myjamong.tistory.com/150)

[추상화클래스와 인터페이스의 용도, 차이점, 공통점[신매력]](https://marobiana.tistory.com/58)

[[JAVA] 자바 추상클래스란?[Limky 삽질블로그]](https://limkydev.tistory.com/188?category=957527)





