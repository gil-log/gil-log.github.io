---
title: "[Java] Exception Handling(복구, 회피, 전환)"
last_modified_at: 2021-09-08T08:26
categories: 
  - java
tags: 
  - 'exception' 
  - 'exception handling' 
  - '예외 복구' 
  - '예외 전환' 
  - '예외 회피'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import

[Java 예외 처리(Handling Exception)에 대해[Nesoy Blog - Que Sera Sera]](https://nesoy.github.io/articles/2018-07/Java-Handling-Exception)
[Java 예외(Exception) 처리에 대한 작은 생각(NEXTREE.IO)](https://www.nextree.co.kr/p3239/)

---


# Exception Handling

**최근 Exception을 Handling** 하면서 **세부적인 Handling 기법들에 대해 고심하지 않고**,

**Handling** 하다보니 **명확한 기준을 잡으며 처리하기 힘들었다.**

**이번 gillog**때 **Exception Handling에 대해 조금 더** 알아보려 한다.

<br>

**먼저 [Error, Exception, Checked, UnChecked](https://velog.io/@gillog/JavaException-Error%EC%9D%98-%EC%B0%A8%EC%9D%B4)에 대한 선행 정리 내용을 숙지한 상태로 시작**해보려 한다.

**아래 Exception Handling 관련 내용**은,

**`UnCheckedException`, `RuntimeException` 관련 Exception Hadnling 기법들**이다.

---

## 1. 예외 복구

**예외 복구의 핵심**은 **Exception이 발생**하여도 **Application은 정상적으로 동작**한 다는 것이다.


**Exception 발생 시 이를 예측**하여 **다른 비즈니스 로직 흐름으로 유도** 시키거나,

**예외가 발생하지 않는 상황으로 복구를 시도하는 로직을 추가**하는 방식이다.


아래 코드를 살펴보자.

```java
private void mayThrowExceptionLogic() {
    int maxTry = 20;
    while(maxTry --> 0) {
    	try {
    		// ???Exception 이 Throw 될 수 가능성이 있는 로직
        
        	// 성공 시 return, 해당 메소드 종료
        	return ;
    	} catch(???Exception e) {
    		// Error 로그 출력
                // 실패 로직 존재 시 원상 복구
        	// 일정 시간 동안 대기
    	} finally {
    		// 작업에 사용한 Resource 반환 및 정리
    	}
    }
    // 최대 횟수 실패시 예외 Throw
    throw new MaxTryFailedException();
}
```

**만약 서버 상태**에 따라 **접속 불안정** 등으로 **실패 가능성이 있는 로직**에,

위 **`maxTry` 만큼 정상 로직을 재시도**하여 **`maxTry`가 초과**될 경우 **또 다른 Exception으로 전환하여 Throw** 한다.



**`maxTry`의 시도**로 **비즈니스 로직이 정상 수행** 될 수 있고,

`throw new MaxTryFailedException();`와 같이 **새로운 Exception으로 전환하여 Throw 하지 않더라도**,

**해당 로직 실패**시 **다른 수행 로직을 첨가**하여 **Application의 흐름을 정상적으로 마무리할 수도** 있다.

---

## 2. 예외 회피

```java
private void mayThrowExceptionLogic() throws ???Exception {
	// 비즈니스 로직
}
```

**위와 같은 코드를 많이 사용**하고 있지만 **아주 신중히 생각**해야 한다.

**Exception 발생 시 `throws`를 통해 호출 된 부분으로 해당 Exception을 던져**버리고,

**Exception Handling을 회피**하는 방식이다.


**해당 방식**은 **호출 부분에서 Exception을 Handling 하는 것이 더 바람직** 하거나,

**해당 로직에서 처리하지않고 Exception을 회피**하고 **throw 하는 것에 확신이 들 경우만 사용해야** 한다.


---

## 3. 예외 전환

```java
try {
	// Exception 발생 가능 로직
}
catch(???Exception e) {
	// Exception 로깅
    	// 복구 로직
        throw CustomSpecificExceptionJustLikeNotAllowedUserException("이런상황~");
}
```

**`예외 전환 방식`**은 **위 코드**와 같이 **특정 Exception 발생 시 새로운 Exception으로 전환**하여,

**호출 부분으로 throw** 하여 **호출 부분에서 Exception을 Handling 할 때**,

**어떤 Exception인지 더욱 분명히**하여 **해당 Exception에 대한 Handling을 더욱 수월히 하는 방식**이다.

<br>


**`CheckedException` 처럼 복구 불가능 Exception을 `catch`** 하여,

**`UncheckedException` 으로 전환**하여 **Handling** 함으로 써,

**다른 계층에서 `CheckedException`을 일일히 선언하지 않아도 되게 할 수도** 있다.





---

## 공통 중요사항

**Exception을 Handling 할 때 중요한 사항들**이 있는데 **아래와 같다.**

### catch에는 로깅, 복구 등의 로직을 추가하기

```java
try {
	// Exception 발생 가능 로직
} catch(???Exception e) {
}
```

**위 코드**와 같이 **Exception을 `catch`한 후에 아무 로직 없이 `catch`만 하는 것은 적합하지 않다.**

**Log를 출력**하거나, **해당 Exception 발생 로직을 원상 복구 시키는 로직을 첨가**하는 등,

**`catch`만 수행하지 않고 해당 Exception에 대한 처리**를 해주어야 한다.

<br>

### catch에 단순히 throw만 하지 않기

```java
try {
	// Exception 발생 가능 로직
} catch(???Exception e) {
	throw e;
}
```

**위 경우와 마찬가지로 Log나 적절한 로직을 추가**하여야 하고,

**단순히 `catch` 후에 `throw`시키는 것은 적합하지 않다.**

<br>

### e.printStackTrace() X, Logging Framework 사용


```java
try {
      // Exception 발생 가능 로직
} catch (IOException e) {
      e.printStackTrace()
}
```

**`Tomcat`에서 `e.printStackTrace()`로 Console에 찍히는 log**는,

**`{TOMCAT_HOME}/logs/catalina.out` 에만 저장**된다.

**`Logging Framework`를 이용하면 파일을 쪼개는 정책을 설정**할 수 있고, 

**여러 Server의 log를 한곳에서 모아서 보는 System을 활용할 수도** 있다.


**`e.printStackTrace()` 대신 `LoggingFramwork`를 활용**하자.
_slf4j, commons logging, log4j, logback ..._

<br>


### Exception Stack을 남겨 추적, 유지보수성 높이기

**Exception의 추적성과 유지보수 성을 높이기 위해**서,

**`e.toString()`이나 `e.getMessage()`로 마지막 Exception Message만 남기기보다**,

**전체 Exception Stack을 다 넘기는 편이 좋다**.
_대표적인 slf4j의 `log.error()` 역시 `e.printStackTrace()`처럼 Exception의 stack도 남긴다._


---


**위에서 정리한 Exception Handling 기법들을 고려**하여 **조금 더 세심한 Handling**을 해보자.