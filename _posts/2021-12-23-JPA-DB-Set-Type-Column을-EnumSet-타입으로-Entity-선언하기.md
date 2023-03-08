---
title: "[JPA] DB Set Type Column을 EnumSet 타입으로 Entity 선언하기"
last_modified_at: 2021-12-23T00:08
categories: 
  - jpa
tags: 
  - 'Converter' 
  - 'Enum' 
  - 'EnumSet' 
  - 'JPA' 
  - 'set'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import

[Mapping EnumSet to mysql Set using JPA 2.1[StackOverFlow]](https://stackoverflow.com/questions/42136578/mapping-enumset-to-mysql-set-using-jpa-2-1/46302681)

---



**기존 MySQL 특정 Table `Column의 Type`**을,

**아래와 같이 `Enum Type`으로 사용**하고 있었다.

```mysql
`CATEGORY` enum('CHINA','KOREA','JAPAN','ALONE','FRANCHISE', 'ETC') DEFAULT 'ETC'
```


```java

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.ToString;

@ToString
@AllArgsConstructor
@Getter
public enum Category {
    CHINA("중식")
    , KOREA("한식")
    , JAPAN("일식")
    , ALONE("1인분")
    , FRANCHISE("프랜차이즈");
    
    private String name;
}

```
**`JPA Entity`를 선언**할 때도 아래와 같이 단순 **`@Enumerated` Annotation을 사용하는 형태**로

**`Enum Type`을 사용**하고 있었다.

```java
@Entity
public class RestaurantEntity {
	...
    
    @Enumerated(EnumType.STRING)
    @Column(name = "CATEGORY")
    private Category category;
    
}
```

프로젝트 도중에 **해당 `Column`의 요구사항이 변경**되어 **기존 `Enum` to `Set` Type**으로,

**`Column Type`을 변경**하게 되었다.
_카테고리는 여러 값을 중복하지 않는 형태로 저장하기 위해 아래와 같은 형태_


![](https://images.velog.io/images/gillog/post/5ffc75df-dded-4bdf-9caa-e54d7da60611/image.png)

```mysql
`CATEGORY` set('CHINA','KOREA','JAPAN','ALONE','FRANCHISE', 'ETC') DEFAULT 'ETC'
```


오늘은 이러한 상황에서 **`JPA Entity`에서 단순 `@Enumerated`로 선언한 요소**를,

**`EnumSet Collection`으로 활용하기 위한 과정을 길록**한다.

---

먼저 **`EnumSet` 사용이 처음이라면 아래 간략 설명을 살펴보고 넘어가자.**
_안봐도 무방_


# EnumSet


**`EnumSet`**은 **Java에서 `Enum` Type**을 **`Set Collection`으로 사용하기 위한 특화 Collection**이다.

**`EnumSet`은 Set Interface를 구현**하고, **`AbstractSet`을 상속**한다. 

이러한 **`EnumSet`은 `Bit Vector`로 구현**되어 있어 **매우 효율적**이다.


> A specialized Set implementation for use with enum types. 
All of the elements in an enum set must come from a single enum type 
that is specified, explicitly or implicitly, 
when the set is created. 
Enum sets are represented internally as bit vectors. 
This representation is extremely compact and efficient.
> [By Java 13 API - EnumSet](https://docs.oracle.com/en/java/javase/13/docs/api/java.base/java/util/EnumSet.html)


![](https://images.velog.io/images/gillog/post/9a8152e8-fbf0-485f-9b1d-399e08915961/image.png)

이러한 **`EnumSet`은 `Thread-Safe` 하지 않아 공유 자원으로 사용시**에는 

**생성 외부에서 `Synchronized`하게 선언**해주어야 한다.

```java
EnumSet<GillogEnum> enumSet = Collections.synchronizedSet(EnumSet.noneOf(GillogEnum.class));
```

---

# @Converter

**`EnumSet` Type으로 JPA Entity에 선언하기 위해서 `Converter`를 사용**해야 한다.


**`@Converter`**는 **`Entity`의 특정 Column에 대한 Data를 변환하여 사용해야 할 때 사용하는 `Annotation`**이다.


<br>

해당 **`@Converter` Annotation이 붙은 Class**는 **`javax.persistence`에서 제공**하는

**`AttributeConverter Interface`를 구현**해야 한다. 

![](https://images.velog.io/images/gillog/post/d8bfba8e-ec1a-4ac6-a2b5-864b60a7cd2c/image.png)


**구현해야 할 `Abstract Method` 두 가지**로

**`convertToDatabaseColumn` method**는 **Entity에서 DB로 입력될 때**,

**Entity에서 선언한 Type `X`**를,

**DB에서 사용할 Type `Y`로 변환해주는 로직을 구현**하면 되고,

<br>

**`convertToEntityAttribute` method**는 **DB에서 Entity로 변환 될 때**,

**DB에서 읽어올 Type `Y`**를, 

**Entity안에 선언한 Type `X`로 변환해주는 로직을 구현**하면 된다.


## AttributeConverter 구현

먼저 **`@Converter`를 사용할 `AttributeConverter Interface`를 구현하는 Class를 생성**해보자.


```java

import lombok.extern.slf4j.Slf4j;
import org.springframework.util.StringUtils;

import javax.persistence.AttributeConverter;
import javax.persistence.Converter;
import java.util.Arrays;
import java.util.EnumSet;

@Slf4j
// 해당 Class가 Converter Class임을 선언
@Converter
// Entity에서 사용할 X Type으로 EnumSet<Category>
// DB에서 읽어오거나 저장할 때 사용할 Y Type으로 String 선언
public class SetCategoryConverter implements AttributeConverter<EnumSet<Category>, String> {

    // DB에서 사용할 Type으로 Entity Type을 변환하는 로직 구현
    @Override
    public String convertToDatabaseColumn(EnumSet<Category> attribute) {
    	// DB에서 사용할 String Type 생성을 위해 StringBuilder 선언
        StringBuilder sb = new StringBuilder();
        // ["KOREA", "ALONE"] 과 같은 EnumSet Collection Data를
        // 각각 foreach를 돌며 "KOREA," 형식으로 StringBuilder에 append
        attribute.stream().forEach(e -> sb.append(e.name()+","));
        // 최종 결과 String으로 변환
        String result = sb.toString();
        // "KOREA,ALONE," 형식 일 경우 마지막 ',' 제거
        if(result.charAt(result.length() - 1) == ',') result = result.substring(0, result.length() - 1);
        return result;
    }

    // Entity에서 사용할 Type으로 DB Type을 변환하는 로직 구현
    @Override
    public EnumSet<Category> convertToEntityAttribute(String dbData) {
        // DB에서 읽어온 값이 null이거나 공백이거나 CATEGORY.KOREA(name="한식") 형태로 읽어올 경우 제외
        if(dbData == null || dbData == "" || dbData.contains(".")) return EnumSet.noneOf(Category.class);
        // 최초 빈 Collection 생성
        EnumSet<Category> attribute = EnumSet.noneOf(Category.class);
        // DB에서 읽어온 "KOREA,ALONE" 형태의 데이터 ','로 split
        String[] dbDataArray = StringUtils.trimAllWhitespace(dbData).toUpperCase().split(",");
        // 빈 Collection으로 생성한 EnumSet에 split한 data를 Category(Enum) .valueOf로 생성
        // 해당 구문에서 Enum에 선언되지 않은 값 존재 시 Exception 발생 가능
        Arrays.stream(dbDataArray).forEach(e -> attribute.add(Category.valueOf(e)));
        return attribute;
    }
}
```

**`@Converter` Class를 자신의 `Enum Type`에 맞게 생성**했다면,

**최종적으로 `Entity` 선언 시에 아래와 같이 `@Convert` Annotation**으로

**해당 Class를 지정**해주면 **DB의 `Set` Column을 Java에서 `EnumSet` Type으로 변환하여 사용 가능**하다.


```java
@Entity
public class RestaurantEntity {

    ...
    
    @Convert(converter = SetCategoryConverter.class)
    @Column(name = "CATEGORY")
    private EnumSet<Category> category;
    
}
```