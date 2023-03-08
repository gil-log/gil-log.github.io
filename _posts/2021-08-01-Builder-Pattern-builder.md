---
title: "Builder Pattern(점층적 생성자 패턴, 자바 빈 패턴 방식과 함께)"
last_modified_at: 2021-08-01T23:55
categories: 
  - java
tags: 
  - 'Builder' 
  - 'Builder Pattern' 
  - 'JavaBeans Pattern' 
  - '점층적 생성자 패턴'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Builder 기반으로 객체를 안전하게 생성하는 방법[cheese10yun]](https://cheese10yun.github.io/spring-builder-pattern/)

[빌더 패턴(Builder Pattern)[기계인간 John Grib]](https://johngrib.github.io/wiki/builder-pattern/)

[Java | Builder패턴 (Lombok @Builder)[개미606]](https://gaemi606.tistory.com/entry/Java-Builder%ED%8C%A8%ED%84%B4-Lombok-Builder)

[[Spring] Lombok을 이용해 Builder 패턴을 만들어보자.[Carry On Programming]](https://zorba91.tistory.com/298)

--- 

# Builder Pattern

**`Builder Pattern`**이란 **복합 객체의 생성 과정**과 **표현 방법을 분리**하여,

**동일한 생성 절차**에서 **서로 다른 표현 결과를 만들 수 있게 하는 Pattern**이다. 

```java
Gillog gillog = Gillog.build()
		.date("21-08-03")
        	.log("builder")
            	.build();
```

**`Builder Pattern`을 사용하는 이유**는 **객체 생성을 깔끔**하고 **유연하게 적용하기 위해 사용**한다.

> **생성자 인자가 많을 때 Builder Pattern 적용을 고려**한다.
 _Effective Java Rule 2_


**`Builder Pattern`을 사용하는 이유**는 **다른 객체 생성 Pattern을 함께 살펴**보면 알아보기 쉽다.


---

## 점층적 생성자 Pattern


**`점층적 생성자 Pattern(Telescoping Constructor Pattern)`**은 **아래와 같은 객체 생성 Pattern**이다.

```java
pulibc class Gillog {
    // 필수 인자
    private final String log;
    
    // 선택 인자
    private final String subject;
    private final String import;

    // 필수 생성자
    public Gillog(String log) {
        this(log, "공통주제", "공통참조");
    }
    
    // 1개 선택 인자 포함 생성자
    public Gillog(String log, String subject) {
    	this(log, subject, "공통참조");
    }
    
    // 모든 선택 인자까지 포함된 생성자
    public Gillog(String log, String subject, String import) {
        this.log = log;
        this.subject = subject;
        this.import = import;
    }
}
```

**`점층적 생성자 Pattern`**은 **아래와 같은 방법으로 생성자를 만드는 Pattern**이다.

1. 필수 인자를 받는 필수 생성자를 생성.

2. 1개의 선택 인자를 받는 생성자를 추가.

3. 2개의 선택 인자를 받는 생성자를 추가.

4. ...

5. 모든 선택 인자를 받는 생성자를 추가.

### 장점

**`new Gillog("점층적 생성자 Pattern", "공통 주제", "공통 참조");`와 같은 객체 생성이 빈번하게 호출**되는 경우,

**`new Gillog("점층적 생성자 Pattern")`으로 간단하게 대체**할 수 있다.

### 단점

1. **다른 생성자를 호출하는 생성자가 많아져, 인자가 추가되는 경우 코드 수정이 힘들다.**

2. **인자 수가 많아질 경우 호출 코드만 봐서는 의미를 알기 힘들어, 코드 가독성이 떨어진다.**


```java
GillogCategory builderPattern = 
	new GillogCategory("BilderPattern", "2021-08-03", 1, 1);
    
GillogCategory telescoptingConstPattern =
	new GillogCategory("TCPattern", 1, 2, 1);
```

---


## JavaBeans Pattern

**`JavaBeans Pattern(자바 빈 패턴)`은 위 `점층적 생성자 패턴`의 대안**으로 나온,

**객체 생성 Pattern으로 `setter method`를 활용**해 **객체 생성 코드를 가독성 있게 만드는 Pattern**이다. 

```java
GillogCategory javaBeansPattern = new GillogCategory();
javaBeansPattern.setLog("자바 빈 패턴");
javaBeansPattern.setSubject("Java");
javaBeansPattern.setViewFlag(1);
javaBeansPattern.setMaxQuestion(2);
javaBeansPattern.setCommentFlag(1);
```

### 장점

`JavaBeans Pattern`으로 **각 인자 의미 파악이 쉬워졌고,**

**복잡한 여러 생성자의 생성이 필요 없게 되었다.**

### 단점

**한번의 호출로 객체 생성이 끝나지 않아, 객체 일관성(consistency)이 깨지게 되었다.**
_생성 객체에 계속해서 값을 추가_

**`setter Method`의 존재로 `immutable Class(변경 불가 클래스)` 생성이 불가능**하다.

---

## Builder Pattern

`Builder Pattern`은


```java
public Class GillogCategory {
    private final String log;
    private final String subject;
    private final int viewFlag;
    private final int commentFlag;
    private final int maxQuestion;
    
    public static class Builder {
    	// 필수 인자
        private final String log;
        private final String subject;
        
        // 선택 인자 기본값 초기화
        private final int viewFlag = 0;
        private final int commnetFlag = 0;
        private final int maxQuestion = 0;
        
        public Builder(String log, String subject) {
            this.log = log;
            this.subject = subject;
        }
        
        public Builder viewFlag(int val) {
            viewFlag = val;
            // 이렇게 return해주어 .으로 Chaining이 가능하다.
            return this;
        }
        
        public Builder commentFlag(int val) {
            commentFlag = val;
            return this;
        }
        
        public Builder maxQuestion(int val) {
            maxQuestion = val;
            return this;
        }
        
        public GillogCategory build() {
            if(viewFlag > 1 || commentFlag > 1) {
            	throw new IllealArgumentException("Flag 값 오인 사용");
            }
            return new GillogCategory(this);
        }
    }
    
    private GillogCategory(Builder builder) {
        log = builder.log;
        subject = builder.subject;
        viewFlag = builder.viewFlag;
        commnetFlag = builder.commentFlag;
        maxQuestion = builder.maxQuestion;
    }
}
```

이제 위 **`Builder Pattern`을 활용한 객체 생성 방법을 아래 코드로 살펴보자.**


```java
// 일반 방식
GillogCategory.Builder builder = 
    new GillogCategory.builder("Builder Pattern", "Object Constructor Pattern");
builder.viewFlag(1);
builder.maxQuestion(4);
builcer.commentFlag(1);
GillogCategory builderPattern = builder.build();

// Chaining Method 방식
GillogCategory builderPattern =
    new GillogCategory.builder("Builder Pattern", "Object Constructor Pattern")
    .viewFlag(1)
    .maxQuestion(4)
    .commentFlag(1)
    .build();
```

### Builder Pattern 장점

1. **객체들마다 들어가야할 인자가 각각 다를 때 유연하게 사용**할 수 있다.


2. **무조건적인 setter 생성을 방지**하고 **불변객체로 생성** 할 수 있다.
_불변성 보장_

3. **필수 인자를 지정할 수 있다.**

4. **인자가 많을 경우 쉽고 안전하게 객체를 생성**할 수 있다.

5. **인자의 순서와 상관없이 객체를 생성**할 수 있다.

6. **적절한 책임을 이름에 부여하여 가독성을 높일 수 있다.**

7. **각 인자의 의미 파악이 쉽다.**

8. **한번에 객체를 생성해 객체 일관성이 깨지지 않는다.**

9. **`build()` Method에 검증 과정을 추가할 수도 있다.**