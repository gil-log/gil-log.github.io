---
title: "[Spring] DI(Dependency Injection) 세 가지 방법"
last_modified_at: 2020-11-08T04:02
categories: 
  - spring
tags: 
  - 'Spring' 
  - 'di'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

앞서 [DI(Dependency Injection)](https://velog.io/@gillog/Spring-DIDependency-Injection)에 대해서 알아보았는데, Spring에서 의존성을 주입하는 세 가지 방법에 대해서 다루어 보려고한다.



`DI`는 Spring에서만 사용되는 용어가 아니라 객체지향 프로그래밍에서는 어디에서나 통용되는 개념이다.

#### 강한 결합
**객체 내부에서 다른 객체를 생성하는 것은 강한 결합도를 가지는 구조**이다. 
A 클래스 내부에서 B 라는 객체를 직접 생성하고 있다면, B 객체를 C 객체로 바꾸고 싶은 경우에 A 클래스도 수정해야 하는 방식이기 때문에 강한 결합이다.

#### 느슨한 결합
**객체를 주입 받는다는 것은 외부에서 생성된 객체를 인터페이스를 통해서 넘겨받는 것**이다.
**이렇게 하면 결합도를 낮출 수 있고, 런타임시에 의존관계가 결정되기 때문에 유연한 구조**를 가진다.

SOLID 원칙에서 O 에 해당하는 Open Closed Principle 을 지키기 위해서 디자인 패턴 중 전략 패턴을 사용하게 되는데, **생성자 주입을 사용하게 되면 전략 패턴을 사용하게 된다.**



**의존성 주입의 종류로는 `Field Injection`, `Setter Injection`, `Constructor Injection` 방법**이 있다.

---

# Field Injection(필드 주입)

변수 선언부에 @Autowired Annotation을 붙인다.


```java
@Component
public class SampleController {
    @Autowired
    private SampleService sampleService;
}

```

## Field Injection을 사용하면 안되는 이유

- **단일 책임(SRP)의 원칙 위반**

**의존성을 주입하기가 쉽다. **
@Autowired 선언 아래 개수 제한 없이 무한정 추가할 수 있으니 말이다. 

여기서 **`Constructor Injection`을 사용하면 다른 Injection 타입에 비해 위기감을 느끼게 해준다. **

**Constructor의 parameter가 많아짐과 동시에 하나의 Class가 많은 책임을 떠안는다는 걸 알게된다. **

이때 이러한 징조들이 Refactoring을 해야한다는 신호가 될 수 있다.

<br>

- **의존성이 숨는다.**

**`DI Container`를 사용한다는 것**은 **Class가 자신의 의존성만 책임진다는게 아니라 제공된 의존성 또한 책임진다. **

그래서** Class가 어떤 의존성을 책임지지 않을 때, 메서드나 생성자를 통해(Setter나 Constructor) 확실히 커뮤니케이션이 되어야한다. **

**하지만 `Field Injection`은 숨은 의존성만 제공**해준다.


<br>

- **DI Container의 결합성과 테스트 용이성**

**`DI Framework`의 핵심** 아이디어는 **관리되는 Class가 `DI Container`에 의존성이 없어야 한다.**

즉, **필요한 의존성을 전달하면 독립적으로 Instance화 할 수 있는 단순 POJO여야한다.** 

**`DI Container` 없이도 Unit Test에서 Instance화** 시킬 수 있고, **각각 나누어서 테스트**도 할 수 있다. 

**`Container`의 결합성이 없다면 관리하거나 관리하지 않는 Class를 사용할 수 있고, 심지어 다른 `DI Container`로 전환할 수 있다.**

하지만, **`Field Injection`을 사용하면 필요한 의존성을 가진 Class를 곧바로 Instance화 시킬 수 없다.**

<br>

- **불변성(Immutability)**

**`Constructor Injection`과 다르게 `Field Injection`은 final을 선언할 수 없다. **
그래서 **객체가 변할 수 있다.**

<br>

- **순환 의존성**

`Constructor Injection`에서 순환 의존성을 가질 경우 BeanCurrentlyCreationExeption을 발생시킴으로써 순환 의존성을 알 수 있다.


#### _순환 의존성?_

A Class가 B Class를 참조하는데 B Class가 다시 A Class를 참조할 경우,
A Class가 B Class를 참조하고, B Class가 C Class를 참조하고 C Class가 A Class를 참조하는 경우 **이를 순환 의존성(Circular Dependency)**이라고 부른다.


<br>

`Field Injection`은 스프링 컨테이너 말고는 외부에서 주입할 수 있는 방법이 없다.


**`Field Injection`은 읽기 쉽고, 사용하기 편하다는 것 말고는 장점이 없다.**

---

# Setter Injection(수정자 주입)

`Setter Injection`은 **선택적인 의존성을 사용할 때 유용**하다. 
**상황에 따라 의존성 주입이 가능**하다. 
스프링 3.x Documents에서는 `Setter Injection`을 추천했었다.

`Setter Injection`은 set Method를 정의해서 사용한다.

```java
@Component
public class SampleController {
    private SampleService sampleService;
 
    @Autowired
    public void setSampleService(SampleService sampleService) {
        this.sampleService = sampleService;
    }
}
```



`Setter Injection`으로 의존관계 주입은 런타임시에 할 수 있도록 낮은 결합도를 가지게 구현되었다.

하지만** `Setter Injection`을 통해서 Service의 구현체를 주입해주지 않아도 Controller 객체는 생성이 가능하다. **

**Controller 객체가 생성가능하다는 것은 내부에 있는 Service의 method 호출이 가능하다는 것**인데,
set을 통해 **Service의 구현체를 주입해주지 않았으므로, NullPointerException 이 발생**한다.

**주입이 필요한 객체가 주입이 되지 않아도 얼마든지 객체를 생성할 수 있다는 것이 문제**다.

이 문제를 해결 할 수 있는 방법이 `Constructor Injection`이다.




# Constructor Injection(생성자 주입)

아래 처럼 Constructor에 @Autowired Annotation을 붙여 의존성을 주입받을 수 있다.

```java
@Component
public class SampleService {
    private SampleDAO sampleDAO;
 
    @Autowired
    public SampleService(SampleDAO sampleDAO) {
        this.sampleDAO = sampleDAO;
    }
}

@Component
public class SampleController {

	private final SampleService sampleService = new SampleService(new SampleDAO());
    
	...
}
```


 

## Construtor Injection을 사용해야 하는 이유


**Spring Framework Reference에서 권장하는 방법은 생성자를 통한 주입**이다.

생성자를 사용하는 방법이 좋은 이유는 **필수적으로 사용해야하는 의존성 없이는 Instance를 만들지 못하도록 강제할 수 있기 때문**이다.

**Spring 4.3버전부터는 Class를 완벽하게 `DI Framework`로부터 분리**할 수 있다. 

**단일 생성자에 한해 @Autowired를 붙이지 않아도 된다.**
_Spring 4.3부터는 클래스의 생성자가 하나이고 그 생성자로 주입받을 객체가 Bean으로 등록되어 있다면 @Autowired를 생략할 수 있다._

또한 앞서 살펴본 **`Field Injection`의 단점들을 장점으로 가져갈 수 있다.**

<br>


- **null을 주입하지 않는 한 NullPointerException 은 발생하지 않는다.**

**의존관계 주입을 하지 않은 경우에는 Controller 객체를 생성할 수 없다. **
즉, 의존관계에 대한 내용을 외부로 노출시킴으로써 컴파일 타임에 오류를 잡아낼 수 있다.

- **final 을 사용할 수 있다. **

final로 선언된 레퍼런스타입 변수는 반드시 선언과 함께 초기화가 되어야 하므로 setter 주입시에는 의존관계 주입을 받을 필드에 final 을 선언할 수 없다.

**final의 장점은 객체가 불변하도록 할 수 있는 점**으로, _누군가가 Controller 내부에서 Service 객체를 바꿔치기 할 수 없다는 점_이다.



- **순환 의존성을 알 수 있다.**

앞서 살펴 본 `Field Injection`에서는 컴파일 단계에서 순환 의존성을 검출할 방법이 없지만, `Construtor Injection`에서는 컴파일 단계에서 순환 의존성을 잡아 낼 수 있다.


- **의존성을 주입하기가 번거로워 위기감을 느낄 수 있다.**

`Construtor Injection`의 경우 생성자의 인자가 많아지면 코드가 길어지며 **개발자로 하여금 위기감을 느끼게 해준다. **

**이를 바탕으로 SRP 원칙을 생각하게 되고, Refactoring을 하게 된다.**





이러한 장점들 때문에 **스프링 4.x Documents에서는 `Constructor Injection`을 권장**한다. 

<br>

굳이 `Setter Injection`을 사용하려면, 합리적인 default를 부여할 수 있고 선택적인(optional) 의존성을 사용할 때만 사용해야한다고 말한다. 
_그렇지 않으면 not-null 체크를 의존성을 사용하는 모든 코드에 구현해야한다._

결국 **더 좋은 디자인 패턴과 코드 품질을 위해서는 `Constructor Injection`을 사용**해야 한다.





<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[스프링 - 생성자 주입을 사용해야 하는 이유, 필드인젝션이 좋지 않은 이유[BY YABOONG]](https://yaboong.github.io/spring/2019/08/29/why-field-injection-is-bad/)

[[Spring] 의존성 주입(DI, Dependency Injection)의 세가지 방법[by 버터필드]](https://atoz-develop.tistory.com/entry/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85DI-Dependency-Injection%EC%9D%98-%EC%84%B8%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)

[[Spring/Core] DI(의존성 주입)은 생성자 주입을 사용해라[효기미나]](https://lee1535.tistory.com/117)

[[Spring]필드 주입(Field Injection) 대신 생성자 주입(Constructor Injection)을 사용해야 하는 이유[Carry On Progamming]](https://zorba91.tistory.com/238)

[]()

[]()