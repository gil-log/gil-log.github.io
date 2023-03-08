---
title: "[Java]Error, Exception(Checked, Unchecked)"
last_modified_at: 2021-05-25T00:36
categories: 
  - java
tags: 
  - 'CheckedException' 
  - 'Java' 
  - 'Rollback' 
  - 'RuntimeException' 
  - 'UncheckedException' 
  - 'error' 
  - 'exception' 
  - 'throwable'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 예외 구분: Checked Exception, Unchecked Exception[오늘도 MadPlay!]](https://madplay.github.io/post/java-checked-unchecked-exceptions)

[Java SE 8 Docs[Oracle]](https://docs.oracle.com/javase/8/docs/api/java/)

---

# Exception, Error

**`Exception`은 정상적인 Program의 flow를 어긋나는 것**을 말한다.
_입력 값에 대한 처리 불가, Program 실행 중 참조된 값이 잘못된 경우 등_

Java에서 **`Exception`은 개발자가 직접 Application Code내에서 처리** 할 수 있어, **예외 상황에 대해 미리 예측하여 Handling**할 수 있다.

<br>

**`Error`는 System에 비정상적인 상황이 발생한 경우**를 말한다.

**Java에서 주로 JVM(Java Virtual Machine)에서 발생시키는 것**이고, **`Exception`과 반대로 Application Code에서 잡아도 처리할 제대로 방법이 없다.**
_`OutOfMemoryError`, `ThreadDeath`, `StackOverflowError` 등_



```java
package src.throwable.error;

public class ErrorExample {

    public static void gillog(String log) {
        System.out.println(log);
    }

    public static void main(String[] args) {
        try {
            gillog("Error Test");
        } catch (StackOverflowError e) {
            // ......... :(
        }
    }
}
```

**위 코드 처럼 `StackOverflowError`에 대해서 Catch를 하려해도 사실 별 소용이 없다.**

---

# Checked Exception, Unchecked Exception


`Exception`과 `Error`가 Java내에서 상속관계를 살펴보면 아래 그림과 같다.
![](https://images.velog.io/images/gillog/post/aef65746-bc68-4215-bb78-c593880e74d9/image.png)
_[[출처]자바 예외 구분: Checked Exception, Unchecked Exception[오늘도 MadPlay!]](https://madplay.github.io/post/java-checked-unchecked-exceptions)_

**`Exception`과 `Error`**는 최상위 `Object`를 상속받는 **`Throwable` Class를 상속받는 Class** 이다.

**`Checked Exception`과 `Unchecked Exception`에 대해 설명 하기전 알아야 할 사항**은 **`Error`와 `Runtime Exception`**이 있다.

**결론적으로 먼저 설명**하면, **`Checked Exception`은 `Error`와 `RuntimeException`을 상속하지 않은 `Exception` Class**를, 
**`Unchekced Exception`은 `RuntimeException`을 상속받는 `Exception` Class를 의미**한다.

<br>

즉, **`RuntimeException`을 상속하지 않고 꼭 처리해야 하는 Exception은 `Checked Exception`**이고,

반대로 **명시적으로 처리하지 않아도 되는 Exception은 `Unchecked Exception`**이다.

### Error

**`Error`는 자식 Class로 `LinkageError`, `ThreadDeath`, `AssertionError`, `VirtualMachineError`**를 가지고 있다.

`Error`를 상속받는 Class에 대한 설명은 아래와 같다.

|Class|설명|
|:--:|:--:|
|LinkageError|**어떤 클래스가 다른 클래스에 대한 종속성이 있는 상황**에서,<br> **후자 클래스가 이전 클래스를 컴파일 한 후 비호환적으로 변경된 경우** 발생하는 Error이다.|
|ThreadDeath|**더 이상 사용되지 않는 Thread**에 대해 **Thread.stop() method가 호출** 될 때, <br> **삭제되는 Thread에서 Instance가 throw 되며 발생하는 Error**이다.|
|AssertionError|**`Assertion`이 실패한 경우 발생하는 Error**이다.<br>_Assertion : 해당 지점에서 개발자가 반드시 참(true)이어야 한다고 생각하는 사항을 표현한 논리식_<br>_[EX] assert [boolean];  assert [boolean] : [표현식];|
|VirtualMachineError|**JVM이 손상**되었거나 **계속 작동하는 데 필요한 리소스가 부족할때 발생하는 Error**|

<br>

### RuntimeException

**`RuntimeException`은 말 그대로 Program 실행 중에 발생**하며 **System 환경적으로나, Input Value가 잘못된 경우, 의도적으로 개발자가 설정한 조건을 위배했을 때 throw** 되게 하는 등, **Application 실행 도중에 발생하는 `Exception`**이다.

**`RuntimeException`은 명시적으로 예외 처리를 하지 않아도 되는 `Exception`**이다.
_Exception 처리를 명시하지 않아도 Program 구동에 아무 문제가 없다._


이제 `CheckedException`과 `UncheckedException`을 조금 자세히 살펴보자.

## CheckedException

**`CheckedException`은 반드시 예외 처리 해야하는 `Exception`**으로, **Compile 시점에서 `Exception` 발생이 확인되는 `Exception`**이다.

**`CheckedException`은 `Error`와 `RuntimeException`을 상속하지 않은 `Exception`들을 모두 포함**한다.
_`Error`, `FileNotFoundException`, `ClassNotFoundException` 등이 대표적이다._

**`Spring Framework`에서 `CheckedException`은 Transaction 처리시에 Exception이 발생해도 Rollback을 하지 않는다.**


## UncheckedException

**`UncheckedException`은 명시적으로 예외 처리 해주지 않아도 되는 `Exception`**으로, **`Runtime` 시점에서 `Exception` 발생이 확인되는 `Exception`**이다.

**`UncheckedException`은 `RuntimeException`을 상속받는 `Exception`들을 포함**한다.
_`NullPointerException`, `ClassCastException` 등_


**`Spring Framework`에서 `UncheckedException`은 Transaction 처리시에 Exception이 발생한 경우 Rollback을 수행**한다.

<br>

`CheckedException`과 `UncheckedException`를 정리한 표는 아래와 같다.

||CheckedException|UncheckedException|
|:--:|:--:|:--:|
|확인 시점|Compile|Runtime|
|처리|반드시 예외 처리|명시적으로 하지 않아도 무관|
|트랜잭션 처리 시<br>_ In Spring Framework_|예외 발생 시 Rollback 수행 X|예외 발생 시 Rollback 수행 O|
|예시|ClassNotFoundException|NullPointerException|




---

