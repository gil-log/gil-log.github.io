---
title: "System Class와 성능 개선"
last_modified_at: 2021-04-29T00:36
categories: 
  - java-performance-tuning-story
tags: 
  - '@property' 
  - 'System Class' 
  - 'System Class Property Methods' 
  - 'environment' 
  - '자바 성능 튜닝 이야기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>


# System Class

**Java를 처음 배울 때 아무 생각없이 쓰는 Class 중 `System` Class가 대표적**이다.

모든 **`System` Class Method static**으로 되어있고, **그 안에서 생성된 `in`, `out`, `err` 객체 들도 static으로 선언**되어 있고, **생성자도 없어 우리는 `System` 객체를 생성할 수 없고, `System.XXX`와 같은 방식으로 사용**해야 한다.

**System Class에서 자주 사용하지 않지만 유용한 메소드**를 알아보면 아래와 같다.

### static void arrayCopy (Object src, int srcPos, Object dest, int destPos, int length)

특정 배열을 복사할 때 사용한다.

**`Object src`는 복사 원본 배열, `Ojbect dest`는 복사한 값이 들어가는 배열**이다.

**`int srcPos`는 원본 시작 위치, `int destPos`는 복사본의 시작 위치 `int length`는 복사하는 개수**이다.



<br>

## 속성과 환경


**Java의 `JVM`에서 사용할 수 있는 설정은 크게 두가지로,`속성(Property)`, `환경(Environment)` 값**이 있다.

**`속성(Property)`은 `JVM`에서 지정된 값들**이고, **`환경(Environment)`은 장비에 지정되어 있는 값들**이다.

Java에서 `속성`은 `Properties`, `환경`은 `env`로 사용한다.

`Properties`를 사용하는 메소드는 아래와 같다.





### static Properties getProperties()

현재 Java `속성` 값들을 받아 온다.



### static String getProperty(String key)

`Key`에 지정된 자바 `속성` 값을 받아 온다.



### static String getProperty(String key, String def)

`key`에 지정된 자바 `속성` 값을 받아 온다.

**`def`는 해당 `key`가 존재하지 않을 경우 지정할 기본 값**이다.



### static void setProperties(Properties props)

**`props` 객체에 담겨 있는 내용을 Java `속성`에 지정**한다.



### static String setProperty(String key, String value)

**Java `속성`에 있는 지정된 key의 값을 value 값으로 변환**한다.

이렇게 Java `속성` 관련 메소드를 어떻게 사용하는지는 아래 예시와 같다.

```java
import java.util.*;
public class GetProperties {
	public static void main(String args[]) {
    	System.setProperty("JavaTuning", "Gillog");
        Properties prop = System.getProperties();
        Set key = prop.keySet();
        
        Iterator iterator = key.iterator();
        while(iterator.hasNext()) {
        	String currentKey = iterator.next().toString();
            System.out.format("%s=%s\n", currrentKey, prop.getProperty(currentKey));
        }
    }
}

```

위 소스 코드는 **`JavaTuning`이라는 `Key`를 갖는 System `Property`에 `Gillog`라는 값을 지정**한 후, **System `Property` 전체 값을 화면에 출력**해 주는 프로그램이다.

이 프로그램을 수행하면 **Java System `Property` 값들을 출력**한다.



### static Map<String, String> getenv()

**현재 System `환경` 값 목록을 String 형태의 Map으로 return** 한다.

### static String getenv(String name)

**name에 지정된 환경 변수의 값을 return**한다.


### static void load(String fileName)

**파일명을 지정하여 Native Library를 로딩**한다.

### static void loadLibrary(String libName)

**Library 이름을 지정하여 Native Library를 로딩**한다.


---

## Waring Methods

**System Class에서 운영중인 코드에 절대 사용해서는 안되는 메소드**가 있다.

### static void gc()

Java에서 **사용하는 메모리를 명시적으로 해제 하도록 GC를 수행하는 메소드**다.

### static void exit(int status)

**현재 수행중인 Java VM을 멈춘다.**

이 메소드는 운영중인 코드에서는 절대 수행하면 안된다.

### static void runFinalization()

**Object 객체에 있는 finalize()라는 메소드는 자동으로 호출**되는데, **GC가 알아서 해당 객체를 더 이상 참조할 필요가 없을때 호출**한다.

하지만 **이 메소드를 직접 호출**하면 **참조 해제 작업을 기다리는 모든 객체의 finalize() 메소드를 일일히 수동으로 수행**해야 한다.
