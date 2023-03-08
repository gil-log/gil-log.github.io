---
title: "Framework vs Library"
last_modified_at: 2020-12-02T23:11
categories: 
  - 개념
tags: 
  - 'framework' 
  - 'library'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
지금까지 `Framework`와 `Library`를 사용하면서 둘의 개념에 대해 모호하게 알고 있었다.
_~~사실 잘 몰랐다.~~_ 

오늘 `Framework`와 `Library`에 대해 알아보려고 한다.

`Framework`와 `Library`에 대한 간단한 그림 설명은 아래와 같다.

![](https://images.velog.io/images/gillog/post/2b5baa7c-59b2-442a-8e67-e0aacb1a02cf/998F9D3359FB43CA12.png)

> 출처 : [프레임워크와 라이브러리의 차이점[망나니개발자]](https://mangkyu.tistory.com/4)


# Framework

`Framework`의 정의는 `소프트웨어의 특정 문제를 해결하기 위해서 상호 협력하는 클래스와 인터페이스의 집합`이다.

**`Framework`는 뼈대나 기반구조**를 뜻하는데, **Application 개발 시 필요한** 필수적인 코드, 알고리즘, 데이터베이스 연동 등과 같은 **기능들을 위해 뼈대(구조)를 제공해주는 것**이다. 

그래서 **`Framework`가 제공하는 구조 위에 프로그래머가 코드를 작성하여 Application을 완성**시켜야 한다.

`Framework`는 제어의 역전 개념이 적용된 대표적인 기술이다.

## 제어의 역전(Inversion Of Control)

**`제어의 역전(IOC)`**이란 **흐름을 강제하는 프레임워크에 제어의 권한을 넘김**으로써 **클라이언트 코드가 신경써야 할 것을 줄이는 전략**이다. 

`Framework`는 실행의 흐름을 `Framework` 자체가 가지고 있어서 **개발자의 코드를 `Framework`안에 넣어서 개발**해야 한다. 

**`Framework`를 사용**하면 **프로그래머가 가지고 있어야하는 제어의 권한을 `Framework`에게** 주었기 때문에 **이를 제어의 역전**이라고 말한다.

## Framework 특징

**특정 개념들의 추상화를 제공하는 여러 클래스나 컴포넌트로 구성**되어 있다.

**추상적인 개념들이 문제를 해결하기 위해 같이 작업하는 방법을 정의**한다. 

**컴포넌트들은 재사용이 가능**하다.

**높은 수준에서 패턴들을 조작화** 할 수 있다.

**이미 완성 된 것이 아닌 개발자가 직접 완성시켜야 한다.**

**객체 지향 개발**에서, **통합성, 일관성의 부족이 발생되는 문제를 해결하는 방법중 하나**이다. 

---

# Library


`Library`의 정의는 `단순 활용이 가능한 도구들의 집합`이다.

**`Library`는 특정 기능에 대한 도구나 함수들을 모은 집합**이다. 

즉, **프로그래머가 개발하는데 필요한 함수들을 모아**, **개발자가 만든 클래스에서 호출하여 사용**, **클래스들의 나열로 필요한 클래스를 불러서 사용하는 방식**이다.


---

# Framework vs Library

**`Framework`와 `Library`의 차이는 Flow(흐름)에 대한 제어 권한이 어디에 있느냐의 차이**다.
_Application의 Flow(흐름)를 누가 쥐고 있느냐_

**`Framework`는 전체적인 흐름을 자체적으로 제어** 하며, **프로그래머가 흐름을 위해 필요한 코드를 작성**하는 반면, 
**`Library`는 사용자가 흐름에 대해 제어**를 하며 **필요한 상황에 가져다 쓰는 것**이다. 

이것이 바로 **`Framework`에서 제어의 역전(Inversion Of Control) 개념**이다.
_[IoC 단순 설명](https://velog.io/@gillog/Spring-DIDependency-Injection)_


<br>


**`Library`를 사용하는 Application 코드는 Application 흐름을 직접 제어**한다.  

그저 동작 중에 **필요한 기능이 있을 때** **능동적으로 `Library`를 사용할 뿐**이다. 

반면 **`Framework`**는** Application 코드**가 **`Framework`가 짜놓은 틀에서 수동적으로 사용되는 것**이다. 


**`Framework` 위에 개발한 클래스를 등록**해두고, **`Framework`가 흐름을 주도하는 중에 개발자가 만든 Application 코드를 사용하도록 만드는 방식**이다.


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️


[프레임워크와 라이브러리의 차이점[망나니개발자]](https://mangkyu.tistory.com/4)

[프레임워크와 라이브러리의 차이점[WebClub KimJaeHee]](https://webclub.tistory.com/458)

[Framework, Library 차이[Dongkyuuuu]](https://hang95-coding.tistory.com/4)

[]()