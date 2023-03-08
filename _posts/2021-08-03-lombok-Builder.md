---
title: "[lombok] @Builder"
last_modified_at: 2021-08-03T00:16
categories: 
  - java
tags: 
  - 'Builder' 
  - 'LomBok'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# @Builder


**`lombok`의 `@Builder`를 Model Class 위에 붙이면 아래와 같은 효과**가 있다.


**`@builder` 사용으로 `Boilerplate Code`를 줄일 수 있다.**
_Boilerplate Code : 반드시 필요한 코드지만 반복적으로 사용되는 코드_





- @Builder 적용 전

```java
@RequiredArgsConstructor
public Class GillogCategory {
    @NonNull
    private final String log;
    @NonNull
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
            return new Gillog(this);
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

- @Builder 적용 후

```java
@Builder
@RequiredArgsConstructor
public Class GillogCategory {
    @NonNull
    private final String log;
    @NonNull
    private final String subject;
    private final int viewFlag;
    private final int commentFlag;
    private final int maxQuestion;
    
}
```
위 **`@RequiredArgsConstructor`**는 **`@NonNull`로 지정된 Field를 매개변수로 하는 필수 생성자를 생성해주는 Annotation**이다.

이를 같이 활용하면 **`@Builder`에서 필수 생성자를 지정해주는 효과를 동일하게 볼 수 있다.**

---

**`@Builder`를 Class위에 선언하면 `@AllArgsConstrutor`를 붙인 것과 같은 효과**를 보는데,

**[Builder Pattern](https://velog.io/@gillog/Builder-Pattern-builder)을 사용할 경우 필수 매개 변수를 최소화 해주는 것이 좋다.**


```java
@RequiredArgsConstructor
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public Class GillogCategory {
    @NonNull
    private final String log;
    @NonNull
    private final String subject;
    private final int viewFlag;
    private final int commentFlag;
    private final int maxQuestion;
    
    @Builder
    public GillogCategory(String log, String subject) {
    	this.log = log;
        this.subject = subject;
    }
}
```

위 **`@NoArgsConstructor(access = AccessLevel.PROTECTED)`를 통해**,

**기본 생성자의 접근 제한자**를 **`JPA`등에서는 접근 가능하지만, 프로젝트 상에서는 직접 호출할 수 없게 제한**할 수 있다.


---

**최종적**으로 아래와 같이 **`@Builder` 사용시**에는,

**접근 제한자를 제한**해주고,

**필수 매개 변수를 지정하여 Class위가 아닌 해당 생성자에 `@Builder`를 붙여주는 것이 바람직하다.**

```java
@Table(name = "GILLOG_CATEGORY")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public Class GillogCategory {
    private final String log;
    private final String subject;
    private final int viewFlag;
    private final int commentFlag;
    private final int maxQuestion;
    
    @Builder(builderClassName = "GillogCategoryBuilder", 
        builderMethodName = "withLogSubject")
    public GillogCategory(String log, String subject) {
    	this.log = log;
        this.subject = subject;
    }
}
```

또한 **위와 같이 `@Builder`에 `builderClassName`과 `builderMethodName`을 지정**해주어,

**해당 Builder의 이름을 명확하게**하여 **적절한 책임을 부여하는 것**이 좋다.

```java
GillogCategory gillog = GillogCategry.withLogSubject()
				.log("21-08-03")
                		.subject("@Builder")
                        	.build();
```