---
title: "JUnit 5"
last_modified_at: 2020-12-08T03:37
categories: 
  - java
tags: 
  - 'Junit 5' 
  - 'TestCase'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
> [백기선님의 live-study 4주차 과제: 제어문 #4](https://github.com/whiteship/live-study/issues/4)

# 4주차 live-study 과제 제어문
## 과제 0. JUnit 5 학습하세요.
인텔리J, 이클립스, VS Code에서 JUnit 5로 테스트 코드 작성하는 방법에 익숙해 질 것.
이미 JUnit 알고 계신분들은 다른 것 아무거나!
[더 자바, 테스트](https://www.inflearn.com/course/the-java-application-test?inst=86d1fbb8) 강의도 있으니 참고하세요~

# 🙆‍♂️ import 🙇‍♂️

[JUnit 5 Docs User Guide](https://junit.org/junit5/docs/current/user-guide/)

[JUnit 5  알아보기[Beom Dev Log]](https://beomseok95.tistory.com/303)

[[JUnit] JUnit5 사용법 - 기본[heejeong Kwon]](https://gmlwjd9405.github.io/2019/11/26/junit5-guide-basic.html)

[JUnit 5 Parameterized Tests 사용하기[dpudpu]](https://dublin-java.tistory.com/56)

[JUnit5의 기능 소개 - 두번째[레이피엘의 블로그]](https://reiphiel.tistory.com/entry/junit5-features2) 

[[JUnit5] @TestInstance[햄과 함께 IT]](https://withhamit.tistory.com/300)

<br>


# JUnit 5

**`JUnit`은 Java용 Unit Test Framework**이다. 

**`JUnit`은 테스트 주도 개발 면에서 중요**하며 `SUnit`과 함께 시작된 **`XUnit`이라는 이름의 Unit Test Framework 계열의 하나**이다.

### XUnit ??

**각각 프로그래밍 언어에서도 단위 테스트를 위한 프레임워크가 존재**하며 대부분 **이름을 xUnit이라 칭한다.**
**자바에서의 Unit Test Framework가 JUnit인 것**이다.

|xUnit|	언어|	관련 사이트|
|:--:|:--:|:--:|
|CUnit|	C|	http://cunit.sourceforge.net/|
|CppUnit|	C++|	https://sourceforge.net/projects/cppunit/|
|PHPUnit|	PHP|	https://phpunit.de/|
|PyUnit	|Python	|http://pyunit.sourceforge.net/|
|JUnit	|Java|	http://junit.org/|

<br>

**`JUnit 5`는** **이전 버전 `JUnit`과 다르게** **세 가지 하위 프로젝트의 여러 모듈로 구성**되어 있다.

**`JUnit 5`는 runtime에 Java 8 이상이 필요**하다. 
_이전 버전의 JDK로 컴파일 된 코드는 계속 테스트 할 수 있다._


---

## JUnit 5 구조


![](https://images.velog.io/images/gillog/post/5f165be7-d6bb-4f99-b36e-bd295485ef26/img1.daumcdn.png)

`JUnit 5`는  `JUnit Platform`, `JUnit Jupiter`, `Junit Vintage`로 구성되어 있다.

### `JUnit Platform` 

**테스트를 발견**하고 **테스트 계획을 생성**하는 **`TestEngine Interface`를 가지고 있다.** 

**`JUnit Platform`은 `TestEngine`을 통해서 테스트를 발견하고, 실행, 실행 결과를 보고**한다.

### `JUnit Jupiter`
**`TestEngine`의 실제 구현체는 별도의 모듈들**인데, **그 모듈 중 하나가 `jupiter-engine`**이다.

**이 모듈은 `jupiter-api`를 사용해서 작성한 테스트 코드를 발견하고 실행**한다.

**`Jupiter API`는 `JUnit 5`에 새롭게 추가된 테스트 코드용 API**로서, 개발자는 **`Jupiter API`를 사용해서 테스트 코드를 작성**할 수 있다.

### `JUnit Vintage` 

**이전 `JUnit 4` 버전으로 작성한 테스트 코드를 실행할 때에는 `vintage-engine` 모듈을 사용**한다.


---

## JUnit 5 사용하기

maven을 이용해서 JUnit 5를 사용하려면 pom.xml에 아래 dependency를 추가해주면 된다.

```
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-params</artifactId>
    <version>5.4.2</version>
    <scope>test</scope>
</dependency>
```

![](https://images.velog.io/images/gillog/post/9ba4c3e5-d7df-437b-a11c-c8abb30b3412/image.png)


성공적으로 `JUnit 5`가 install 되었다.

---

## JUnit assert Method


|Method	|내용|
|:--:|:--:|
|assertArrayEquals(a,b)	|배열 a와b가 일치하는지 확인한다.|
|assertEquals(a,b)	|객체 a와b의 값이 같은지 확인한다.|
|assertSame(a,b)	|객체 a와b가 같은 객체인지 확인한다.|
|assertTrue(a)	|a가 참인지 확인한다.|
|assertFalse(a)|a가 거짓인지 확인한다.|
|assertNotNull(a)|a객체가 Null이 아님을 확인한다.|
|assertThrows(CustomException.class, () -><br> new CustomClass(-1))|첫번째 인자로 발생할 예외 클래스의 Class 타입을 받는다.|

---

## JUnit 5 Annotations

### @Test

해당 Method는 Test대상 메소드임을 의미한다.

### @ParameterizedTest

`@ValueSource`와 같이 사용되며 다른 테스트와 동일하다.
```java
  @ParameterizedTest
  @ValueSource(ints = {1, 3, 5, -3, 15, Integer.MAX_VALUE}) // 6개 인수
  void isOdd_ShouldReturnTrueForOddNumbers(int number) {
      assertTrue(Numbers.isOdd(number));
  }
```

### @ValueSource

해당 annotation에 지정한 배열을 파라미터 값으로 순서대로 넘겨준다.

test method 실행 당 하나의 인수(argument)만을 전달할 때 사용할 수 있다.

리터럴 값의 배열을 테스트 메서드에 전달한다.

#### literal values 종류
short, byte, int, long, float, double, char, java.lang.String, java.lang.Class


```java
@ParameterizedTest
@ValueSource(strings = {"", "  "})
void isBlank_ShouldReturnTrueForNullOrBlankStrings(String input) {
    assertTrue(Strings.isBlank(input));
}

```


### @RepeatedTest

동일 Test를 반복할 때 사용한다.

```java
@TestFactory
@RepeatedTest(4)
void repeatedTest() {
    String testStr = null;
    assertThrows(NullPointerException.class, () -> {
    testStr.length();
    });
}
```


### @TestMethodOrder

`@TestMethodOrder`은 **JUnit 5.4버전부터 제공된 테스트의 순서를 지정할 수 있는 Annotation**이다.
일반적으로 테스트는 순서에 의존하지 않도록 작성하는 것이 유지보수 측면에서 바람직하다.

하지만 **로직 흐름을 순서대로 테스트하는 것이 테스트 코드 작성에 편할 경우에 사용**한다.

JUnit 이전 버전에서 제공되던 `@FixMethodOrder` Annotation은 메소드 이름에 의한 순서만 지정할 수 있어서 테스트 메소드 앞에 알파벳을 넣어서 순서를 지정하거나 하는 불편한 방법을 취했었다.

새롭게 추가된 `@TestMethodOrder` Annotation은 알파벳 순서 이외에도 Annotation으로 직접 순서를 지정하거나 랜덤한 순서로 실행하는 기능을 제공한다.

```java
@TestMethodOrder(OrderAnnotation.class)
class TestMethodOrderExample {

    @Order(3)
    @Test
    void test1() {
        System.out.println("test");
    }

    @Order(1)
    @Test
    void test3() {
        System.out.println("test3");
    }

    @Nested
    @TestMethodOrder(OrderAnnotation.class)
    class NestedTest {

        @Order(4)
        @Test
        void test2() {
            System.out.println("test2");
        }

        @Order(2)
        @Test
        void test4() {
            System.out.println("test4");
        }
    }
}
```

![](https://images.velog.io/images/gillog/post/87daa646-71e0-4223-a826-dc656b830322/image.png)


### @DisplayName

테스트 클래스 또는 테스트 메서드의 이름을 정의할 수 있다.

```java
@DisplayName("Single test successful")
@Test
void testSingleSuccessTest() {
    log.info("Success");
}
```

### @Disabled

이전 버전의 `@Ignore`와 동일하게, 테스트 클래스 또는 메서드를 비활성화할 수 있다

```java
@Test
@Disabled("Not implemented yet")
void testDiabled() {
}
```
### @BeforeAll

이전 버전의 `@BeforeClass`와 동일하게, 현재 클래스의 모든 테스트 메서드보다 먼저 실행된다.
해당 메서드는 static 이어야 한다.

```java
@BeforeAll
static void beforeStart() {
    System.out.println("처음 시작 전.");
}
```
![](https://images.velog.io/images/gillog/post/e329789e-bc96-4235-a6c6-92a1385d742f/image.png)

### @BeforeEach

`@BeforeEach`이 달린 메서드가 각 테스트 메서드 전에 실행된다.

이전 버전의 `@Before`와 동일하다.

```java
@BeforeEach
void init() {
    System.out.println("각 메소드 실행 전 시작.");
}
```


![](https://images.velog.io/images/gillog/post/fdcacb8b-e58e-4541-bd1a-2f85a23617c5/image.png)

### @AfterAll

`@AfterAll`이 달린 메서드는 현재 클래스의 모든 테스트 메소드보다 이후에 실행된다.

해당 메서드는 static이어야 한다.

이전버전의 `@AfterClass` 와 동일하다.

```java
@AfterAll
static void finalDone() {
    System.out.println("최종 완료.");
}
```

![](https://images.velog.io/images/gillog/post/22b091d7-5b44-430e-8496-4810d48eda76/image.png)

### @AfterEach
이전 버전의 `@After`와 동일하게, 각 테스트 메서드 실행 이후에 실행된다.

```java
@AfterEach
void done() {
    System.out.println("각 메소드 실행 후 시작.");
}
```
![](https://images.velog.io/images/gillog/post/fde39b0f-3716-42de-b5dd-b19cc982d572/image.png)


## ETC Annotations


### @Nested

주석이 달린 클래스는 정적이 아닌 중첩 테스트 클래스임을 나타낸다.

### @Tag

테스트 필터링을 위한 태그를 선언할 수 있다.


### @Timeout

실행에 주어진 시간을 초과하는 경우 테스트, 테스트 팩토리, 테스트 템플릿 또는 수명주기 방법이 실패한다.

### @ExtendWith

사용자 정의 확장명을 등록하는 데 사용된다

### @TestFactory
동적으로 테스트를 작성할 수 있게 해준다. 

`@ParameterizedTest`와 비슷하지만 보다 유연하게 테스트를 작성할 수 있다.

### @TestTemplate

여러 번 호출되도록 설계된 테스트 케이스의 템플릿임을 나타낸다.


### @TestInstance

`@TestInstance`는 테스트 인스턴스의 라이프 사이클을 설정할 때 사용한다.

- PER_METHOD : test 함수 당 인스턴스가 생성된다.

- PER_CLASS : test 클래스 당 인스턴스가 생성된다.
```java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class Test() {

    @BeforeAll
    fun setUp(){
    	println("setUp")
    }

    @AfterAll
    fun clean(){
    	println("clean")
    }

    @Test
    fun test() {
    
    }
}
```
위와 같이 `@TestInstance(TestInstance.Lifecycle.PER_CLASS)` Annotation으로 라이프 사이클을 클래스 단위로 설정할 수 있다.

라이프 사이클을 클래스 단위로 지정해 놓으면 `@BeforeAll`, `@AfterAll` Annotation을 static method가 아닌 곳에서도 사용할 수 있다.

따라서 static method가 없는 kotlin에서도 위와 같이 일반 함수에 `@BeforeAll`, `@AfterAll` Annotation을 사용할 수 있다.

---


