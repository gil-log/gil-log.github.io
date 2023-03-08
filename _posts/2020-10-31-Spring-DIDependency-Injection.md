---
title: "[Spring] DI, IoC 정리"
last_modified_at: 2020-10-31T04:40
categories: 
  - spring
tags: 
  - 'IoC' 
  - 'Spring' 
  - 'di'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# DI(Dependency Injection)


`DI(Dependency Injection)`란 **스프링이 다른 프레임워크와 차별화되어 제공하는 의존 관계 주입 기능**으로,
**객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입 시켜주는 방식**이다.


**DI(의존성 주입)를 통해서 모듈 간의 결합도가 낮아지고 유연성이 높아진다.**



![](https://images.velog.io/images/gillog/post/08489bda-549e-4dae-851b-8ae1734bf85e/21373937580AEF9B37.jpg)



첫번째 방법은 A객체가 B와 C객체를 New 생성자를 통해서 직접 생성하는 방법이고, 

두번째 방법은 **외부에서 생성 된 객체를 setter()를 통해 사용하는 방법**이다. 

이러한 두번째 방식이 의존성 주입의 예시인데,
`A 객체`에서 **`B, C객체`를 사용(의존)할 때** `A 객체`에서 **직접 생성 하는 것이 아니라** **`외부(IOC컨테이너)`에서 생성된 `B, C객체`를 조립(주입)시켜 `setter` 혹은 `생성자`를 통해 사용하는 방식**이다.





![](https://images.velog.io/images/gillog/post/41f2eb24-fce2-4b7e-b9ac-d5c3ce97d213/22535642580C4AF12C.jpg)









**스프링에서는 객체를 `Bean`**이라고 부르며, 프로젝트가 실행될때 사용자가 Bean으로 관리하는 객체들의 생성과 소멸에 관련된 작업을 자동적으로 수행해주는데 객체가 생성되는 곳을 스프링에서는 Bean 컨테이너라고 부른다.










# Ioc(Inversion of Control)









`IoC(Inversion of Control)`란 "제어의 역전" 이라는 의미로, 말 그대로 **메소드나 객체의 호출작업을 개발자가 결정하는 것이 아니라, 외부에서 결정되는 것을 의미**한다.

`IoC`는 **제어의 역전이라고 말하며, 간단히 말해 "제어의 흐름을 바꾼다"**라고 한다.

객체의 **의존성을 역전시켜 객체 간의 결합도를 줄이고 유연한 코드를 작성**할 수 있게 하여 **가독성 및 코드 중복, 유지 보수를 편하게** 할 수 있게 한다.



기존에는 다음과 순서로 객체가 만들어지고 실행되었다.


1. 객체 생성

2. 의존성 객체 생성
_클래스 내부에서 생성_

3. 의존성 객체 메소드 호출 



하지만, 스프링에서는 다음과 같은 순서로 객체가 만들어지고 실행된다.


1. 객체 생성

2. 의존성 객체 주입
_스스로가 만드는것이 아니라 제어권을 **스프링에게 위임하여 스프링이 만들어놓은 객체를 주입**한다._

3. 의존성 객체 메소드 호출



**스프링이 모든 의존성 객체를 스프링이 실행될때 다 만들어주고 필요한곳에 주입**시켜줌으로써 **Bean들은 `싱글턴 패턴`의 특징**을 가지며, 

**제어의 흐름을 사용자가 컨트롤 하는 것이 아니라 스프링에게 맡겨 작업을 처리**하게 된다. 








<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[Spring - Spring을 왜 사용하나요?(DI)](https://galid1.tistory.com/493?category=769011)

[스프링(Spring) - DI(Depedency Injection) 개념과 예제[공부해서 남 주자]](https://private.tistory.com/39?category=655784)

[스프링(Spring) DI와 IOC에 대해[henjun's dev journal]](https://cofived.tistory.com/39)

[[Spring] IoC, DI 란?](https://jobc.tistory.com/30)

[[스프링] IoC(Inversion of Control), DI(Dependency Injection), Spring Container, Bean 정리[꾸준하게]](https://leveloper.tistory.com/33)

[]()