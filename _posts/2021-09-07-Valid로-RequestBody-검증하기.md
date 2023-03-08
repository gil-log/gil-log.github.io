---
title: "@Valid로 @RequestBody 검증하기"
last_modified_at: 2021-09-07T08:30
categories: 
  - java
tags: 
  - '@Valid' 
  - 'RequestBody' 
  - 'validator' 
  - '검증'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import

[@Valid 를 이용해 @RequestBody 객체 검증하기[쟈미의 devlog]](https://jyami.tistory.com/55)

[Spring Boot의 @Valid 어노테이션이 동작하지 않는 이슈[Chasing Yesterday - Jinhong]](https://xlffm3.github.io/spring%20&%20spring%20boot/Valid/)

[Spring MethodArgumentNotValidException (@Valid 예외처리)[BrantHwang]](https://medium.com/chequer/spring-methodargumentnotvalidexception-valid-%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC-2f63e8087759)


[@Valid 세팅 및 사용법[Simple is Best!]](https://cchoimin.tistory.com/entry/Valid-%EC%84%B8%ED%8C%85-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95)

---

# @Valid

**`@Valid` Annotation**은 **`javax.validation`에 포함된 Dependency**로,

**`@RequestBody` Annotation으로 Mapping**되는 **Java 객체**의 **유효성 검증을 수행하는 Annotation**이다.

---

# Dependency 추가하기

**해당 글**은 **Maven을 통해 `pom.xml`에 추가하는 방식**으로,

**Dependency를 추가하는 방법을 설명**한다.



**`pom.xml`에 아래 내용을 추가**하자.

```
<dependency>
	<groupId>org.hibernate.validator</groupId>
	<artifactId>hibernate-validator</artifactId>
	<version>6.2.0.Final</version>
</dependency>
```


## 주의사항!!
아래 **groupId가 `javax.validation`인 dependency**는 **현재 Spring 최신 버전들에서 지원되지 않고 있다.**

만약 **아래 
_`<groupId>javax.validation</groupId>`_로 `pom.xml`에 추가해둔 상태라면**,

위 **`<groupId>org.hibernate.validator</groupId>`를 사용**하자.

_본인은 이것 때문에 꽤 애먹었다._


---

# 사용하기


**먼저 간단하게 사용법**을 알아보면,

아래 세 가지 과정을 거치면 된다.

## 1. 검증 객체에 Constraints Annotation 추가


**유효성 검증을 원하는 객체**에 아래와 같이 **`javax.validation.constraints`에 있는 `Annotation`을 객체 Field에 선언**해주자.

_관련 Annotation에 대한 설명은 아래에서 설명_

```java
import java.util.Date;
import javax.validation.constraints.NotNull;
import project.enums.jwt.JWTRoles;

public class JWTBody {
	private String subject;
	private String issuer;
	@NotNull(message = "UserID는 필수 값입니다.")
	private String userId;
	private String platformId;
	private JWTRoles role;
	private Date expireDate;

```

## 2. Controller Parameter @RequestBody 옆에 @Valid Annotation 추가


**`@RequestBody`를 통해 Binding**하는 **Controller의 method parameter에서**,

**유효성 검증을 원하는 해당 객체 옆**에 **`@Valid` Annotation을 추가**하자.


```java
import javax.validation.Valid;
import org.apache.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import project.model.auth.JWTBody;
import project.model.common.CommonResponse;
import project.service.auth.AuthService;

@RequestMapping("tokens")
@RestController
public class TokenController {
	@SuppressWarnings("unused")
	private static final Logger LOG = Logger.getLogger(TokenController.class);
	@Autowired
	private AuthService authService;

	@PostMapping("")
	public ResponseEntity<CommonResponse<String>> createToken(@RequestBody @Valid JWTBody jwtBody) {
		return ResponseEntity.ok(new CommonResponse<>(authService.createJWT(jwtBody)));
	}
}

```

## 3. MethodArgumentNotValidException Exception Handler 추가

이제 **유효성 검증을 원하는 객체에 선언**한 **`Constraint Annotation`에 따라**서,

**유효성 검증에 실패**할 경우 **`MethodArgumentNotValidException`이 발생**하는데 **이 Exception을 Handling**해주자.


**본인은 현재 `ControllerAdvice`를 통해 Handling** 하는중이다.
```java
import javax.servlet.http.HttpServletRequest;
import org.apache.log4j.Logger;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice
public class ExceptionController {
	@SuppressWarnings("unused")
	private static final Logger LOG = Logger.getLogger(ExceptionController.class);

	@ExceptionHandler(MethodArgumentNotValidException.class)
	protected Object handleMethodArgumentNotValidException(MethodArgumentNotValidException e, HttpServletRequest request) {
		return e.getBindingResult().getAllErrors();
	}
}
```


## Error 내용 확인해보기

이제 **해당 객체에 유효하지 않은 @RequestBody를 요청**하면,

![](https://images.velog.io/images/gillog/post/cd5de479-a62e-4de7-bd86-b03c8ef793b4/image.png)

아래와 같이 **유효성을 설정한대로 Error 내용을 확인**할 수 있다.

---



# Constraints Annotation


좀 더 **유연한 유효성 검증**을 위해 **`Validator`에서 제공**하는,

**`Constraints Annotation`에 대해 설명**하면 **검증 유형 별로 아래**와 같다.

---

## 문자열 검증(@NotNull, @NotEmpty, @NotBlank, @Null)

### @NotBlank

**null 이 아닌 값**으로, **공백이 아닌 문자를 하나 이상 포함**한다.

**반드시 값이 존재**하고 **공백 문자를 제외**한 **길이가 0보다 커야** 한다.

```java
@NotBlank(message = "이름은 필수 값 입니다.")
private String name;
```

### @NotEmpty

**null 이거나 empty(빈 문자열)가 아닌 값**.

**반드시 값이 존재**하고 **길이 혹은 크기가 0보다 커야**한다.

```java
@NotEmpty(message = "이름은 필수 값 입니다.")
private String name;
```



### @NotNull


**null 이 아닌 어떤 타입이든 수용** 한다.

**반드시 값**이 있어야 한다.


```java
@NotNull(message = "이름은 필수 값 입니다.")
private String name;
```

### @Null

**어떤 타입이든 수용하며 Null**이어야 한다.

<br>

여기서 **문자열에 Null을 허용하지 않는**,

**`@NotNull`, `@NotEmpty`, `@NotBlank`를 비교**해보면 아래와 같다.

|Annotation|null|""|" "|
|:--:|:--:|:--:|:--:|
|@NotNull|	**Invalid**	|Valid|	Valid|
|@NotEmpty|	**Invalid**|	**Invalid**	|Valid|
|@NotBlank	|**Invalid**|	**Invalid**	|**Invalid**|

---

## 최대 최소 검증(@Max, @Min, @DecimalMax, @DecimalMin)

### @Max
**지정된 최대 값보다 작거나 같아야** 한다.

**Annotation에 필수적**으로 **int value로 max 값을 지정**한다.

```java
@Max(value = 99999)
private int max;
```

### @Min

**지정된 최소 값보다 크거나 같아야** 한다.

**Annotation에 필수적**으로 **int value로 min 값을 지정**한다.


```java
@Min(value = 1)
private int min;
```

### @DecimalMax

**지정된 최대 값보다 작거나 같아야** 한다.

**Annotation에 필수적**으로 **String value로 max 값을 지정**한다.

```java
@DecimalMax(value = "999999")
private BigInteger decimalMax;
```


### @DecimalMin 

**지정된 최소 값보다 크거나 같아야** 한다.

**Annotation에 필수적**으로 **String value로 min 값을 지정**한다.

```java

@DecimalMin(value = "1")
private BigInteger decimalMin;
```
<br>


**해당 Annotation들이 적용 가능한 Type**은,

**`BigDecimal`, `BigInteger`, `CharSequence`, `byte`, `short`, `int`, `long`과 이에 대응하는 Wrapper Class 들**이다.


_`double`, `float`는 `Rounding Error` 때문에 지원 X_

**null도 valid**로 간주된다.


<br>

여기서 **`@DecimalMax`, `@DecimalMin`**과,

**`@Max`, `@Min`의 차이**는 **범위 값의 차이**이다. 

@.*?max(value = String or Integer)

**`value` property에 String을 사용**하느냐,

**Integer를 사용**하느냐에 따라 **범위 값이 현저히 달라지기 때문**이다.


---

## 범위 값 검증(@Positive, @PositiveOrZero, @Negative, @NegativeOrZero)



### @Positive

**양수 값만 가능**하다.

```java
@Positive
private int positive;
```

### @PositiveOrZero

**양수 거나 0인 값만 가능**하다.

```java
@PositiveOrZero
private int positiveOrZero;
```




### @Negative

**음수 값만 가능**하다.

```java
@Negative
private int negative;
```

### @NegativeOrZero

**음수 거나 0인 값만 가능**하다.

```java
@NegativeOrZero
private int negativeOrZero;
```


<br>

**해당 Annotation들이 적용 가능한 Type**은,


**`BigDecimal`, `BigInteger`, `CharSequence`, `byte`, `short`, `int`, `long`, `double`, `float`과 이에 대응하는 Wrapper Class 들**이다.


**null도 valid**로 간주된다.


---


## 시간 검증(@Future, @FutureOrPresent, @Past, @PastOrPresent)


### @Future

**현재보다 미래의 날짜, 시간만 가능**하다.

```java
@Future
private Date future;
```

### @FutureOrPresent

**현재거나 미래의 날짜, 시간만 가능**하다.

```java
@FutureOrPresent
private Date futureOrPresent;
```


### @Past

**현재보다 과거의 날짜, 시간만 가능**하다.

```java
@Past
private Date past;
```

### @PastOrPresent

**현재거나 과거의 날짜, 시간만 가능**하다.

```java
@PastOrPresent
private Date pastOrPresent;
```


<br>

**해당 `Annotation`들이 적용 가능한 Type은 아래**와 같다.

- java.util.Date java.util.Calendar
- java.time.Instant java.time.LocalDate 
- java.time.LocalDateTime java.time.LocalTime 
- java.time.MonthDay java.time.OffsetDateTime 
- java.time.OffsetTime java.time.Year 
- java.time.YearMonth java.time.ZonedDateTime 
- java.time.chrono.HijrahDate 
- java.time.chrono.JapaneseDate 
- java.time.chrono.MinguoDate 
- java.time.chrono.ThaiBuddhistDate


**null도 valid로 간주**된다.

<br>

**현재의 기준**은 **`ClockProvider`의 가상 머신에 따라 정의**하며,

**필요한 경우 default time zone을 적용**한다.

---

## 이메일 검증(@Email)

### @Email

**@가 포함된 올바른 이메일 양식만 가능**하다.


```java
@Email
private String email;
```

---


## 자릿수 검증(@Digits)

### @Digits

**허용 범위 내의 숫자 값만 가능**하다.

`@Digits`는 **`int integer` property**로 **허용되는 최대 정수 자릿 수를 설정**하고,

**`int fraction` property**로 **허용되는 최대 소수 자릿 수를 설정**한다.



```java
@Digits(integer = 3, fraction = 4)
private Integer digits;
```

<br>

**해당 `Annotation`이 적용가능한 Type**은,

**`BigDecimal`, `BigInteger`, `CharSequence`, `byte`, `short`, `int`, `long`과 이에 대응하는 Wrapper Class**이다.



**null도 valid로 간주**된다.

---

## Boolean 검증(@AssertTrue, @AssertFalse)

### @AssertTrue

**항상 True인 값**이어야 한다.

```java
@AssertTrue
private boolean true;
```


### @AssertFalse

**항상 False인 값**이어야 한다.

```java
@AssertFalse
private boolean false;
```


**해당 `Annotation`이 적용가능한 Type**은, 
**`Boolean`, `boolean`** 이다.


---

## 크기 검증(@Size)

**`max`, `min` property로 설정한 경계(포함)에 있는 크기의 값**이어야 한다.

**`int max` property보다 크기가 작거나 같아야** 하고,

**`int min` property보다 크거나 같아야** 한다.

_해당 **property 들은 필수**_


```java
@Size(max = 3, min = 1)
private String size;
```

**해당 `Annotation`이 적용가능한 Type**은, 

**`CharSequence (character sequence의 length)`,
`Collection (collection 의 size)`,
`Map (map 의 size)`,
`Array (array 의 length)` 이다.**

**null도 valid로 간주**된다.


---

## 정규식 검증(@Pattern)

**지정한 정규식에 대응되는 문자열 값만 가능**하다.

**`String regexp`는 필수 property로 정규식 문자열을 지정**한다.

_Java Pattern Pacakge Convension을 따름_

```java
@Pattern(regexp = "^.*?([0-9]+)[\?|!]$")
private String pattern;
```


**해당 `Annotation`이 적용가능한 Type**은,

**`CharSequence`**이다.



**null도 valid로 간주**된다.

[정규 표현식 설명](https://velog.io/@gillog/Regular-Expression)

