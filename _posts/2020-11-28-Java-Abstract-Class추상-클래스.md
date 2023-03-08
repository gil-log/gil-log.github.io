---
title: "[Java] Abstract Class(추상 클래스)"
last_modified_at: 2020-11-28T11:12
categories: 
  - java
tags: 
  - 'Java' 
  - 'abstract class'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# Abstract Class(추상 클래스)

**`Abstract Class`**란 **구체적이지 않은 Class를 의미**한다. 
_[EX] 독수리, 타조는 구체적인 새를 지칭하는데 새, 포유류 같은 것은 구체적이지 않다._

주로 **클래스들의 공통되는 필드와 메소드를 정의한 클래스**를 말한다.

이런 것을 구현한 클래스를 `Abstract Class`라고 한다.


# Abstract Class 특징

**추상 클래스는 클래스 앞에 `abstract` 키워드를 이용해서 정의**한다.

**추상 클래스는 미완성의 추상 메소드를 포함**할 수 있다.
_추상 메소드란, **내용이 없는 메소드** 이다. 
즉 **구현이 되지 않은 메소드**이다._

**추상 메소드는 리턴 타입 앞에 `abstract`라는 키워드**를 붙여야 한다.

**추상 클래스는 인스턴스를 생성할 수 없다.**
_자체적으로 객체를 생성할 수 없다. 
따라서 상속을 통해 자식 클래스에서 인스턴스를 생성해야 한다._

**일반적인 상속의 특성과 동일**하다.
_extends 이용, 단일 상속, 생성자 호출 등_

```java
public abstract class GilAbstractClass{
    public abstract void think();

    public void logging(){
        System.out.println("블로깅");
    }
}
```
<br>

**추상 클래스를 상속받은 클래스**는 **추상 클래스가 갖고 있는 추상 메소드를 반드시 구현(Overriding)**해야 한다.

추상 클래스를 상속받고, **추상 클래스가 갖고 있는 추상 메소드를 구현하지 않으면 해당 클래스도 추상 클래스**가 된다.

```java
public class TodayGilLog extends GilAbstractClass{
    
    // 추상 메소드를 구현하지 않으면
    // TodayGilLog Class도 추상 클래스가 된다.
    @Override
    public void think() {
        System.out.println("오늘은 뭐하지");
    }
}
```

## Abstract Class는 객체를 생성할 수 없다.

```java
public class AbstractClassExam { 
    public static void main(String[] args) {
        TodayGilLog todayGilLog = new TodayGilLog();
        todayGilLog.think();
        todayGilLog.logging();
        
        // 추상 클래스는 인스턴스화 시킬 수 없다.
        //GilAbstractClass abstractClass = new GilAbstractClass();
    }   
}
```


# Abstract Class 사용 이유

## 공통 필드와 메소드 통일 목적

`Abstract Class`는 작은 프로젝트에서 사용하는 일은 거의 없을 것이다. 

하지만 **프로젝트가 커지면 여러 개발자가 참여**하게 되는데,  이때 **공통적으로 작성되어야 하는 내용들이 생기게 된다.** 

이러한 내용들을 개발자들마다 이름(메소드명, 필드명 등)을 다르게 정의한다면 **유지보수 및 관리 등 문제가 발생**하게 된다.

따라서 **`Abstract Class`를 사용함으로써 공통된 내용(메소드, 필드)들을 추출하여 통일된 내용으로 작성하도록 규격화하는 것**이다.

**상속받은 클래스들은 자기 클래스의 필요한 메소드나 필드만 추가로 정의**하고, **추상 메소드를 오버라이딩하여 클래스마다 다르게 실행될 로직을 작성**해 주면 된다.

따라서, **필드와 메서드 이름을 통일하여 유지보수성을 높이고 통일성을 유지**할 수 있다.

## 실체클래스 구현시, 시간절약

**실제 프로젝트에서 AA(Application Architecture)가 설계해 놓은 추상 클래스를 상속**받으면, **프로젝트에서 공통적으로 들어가야하는 필드와 메서드가 오버라이딩** 된다. 

즉, **강제로 주어지는 필드와 메서드를 가지고 자신만의 스타일대로 구현만 하면 된다. **

**설계 시간이 절약되고, 구현에만 집중**할 수 있게 된다.


## 객체지향적인 설계

소스 수정시 **다른 소스의 영향도를 적게 가져가면서 변화에는 유연하게 만들기 위해 추상클래스를 사용**하기도 한다. 

**추상클래스를 상속받아서 미리 정의된 공통 기능들을 구현**하고, 실체클래스에서 필요한 기능들을 **클래스별로 확장 시킬 수 있다.**

**추상 클래스를 상속받은 실체 클래스들**은 **반드시 추상메서드를 재정의(Overriding)해서 실행 내용을 작성해야 한다. **

**코드 수정시, 영향도를 적게 가져가면서 유지보수성을 높일 수 있다.** 
_선임 설계개발자가 와꾸(?)를 잡아주고, 후임 개발자는 해당 규격에 맞게 클래스를 구현_ 

 


**규격에 맞게 소스가 구현되어 있기 때문에 해당 규격에 대한 구현부만 수정하면 손 쉽게 기능 수정이 가능하기 때문**이다.



<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️


[자바 입문>추상클래스[Programmers]](https://programmers.co.kr/learn/courses/5/lessons/188)

[[JAVA/자바] 추상 클래스(abstract class), 추상 메소드[JOKER's ROOM]](https://m.blog.naver.com/PostView.nhn?blogId=heartflow89&logNo=220963055326&proxyReferer=https:%2F%2Fwww.google.com%2F)

[자바의 추상 클래스와 인터페이스 - 추상 클래스와 인터페이스의 차이[by강관우]](https://brunch.co.kr/@kd4/6)

[[JAVA] 추상클래스 VS 인터페이스 왜 사용할까? 차이점, 예제로 확인 [마이자몽 myJamong]](https://myjamong.tistory.com/150)

[추상화클래스와 인터페이스의 용도, 차이점, 공통점[신매력]](https://marobiana.tistory.com/58)

[[JAVA] 자바 추상클래스란?[Limky 삽질블로그]](https://limkydev.tistory.com/188?category=957527)