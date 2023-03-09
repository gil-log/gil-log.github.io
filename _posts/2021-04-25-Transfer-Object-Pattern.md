---
title: "Transfer Object Pattern"
last_modified_at: 2021-04-25T09:57
categories: 
  - java-performance-tuning-story
tags: 
  - 'Design Pattern' 
  - 'Transfer Object Pattern' 
  - 'pattern' 
  - '자바 성능 튜닝 이야기'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>


# 디자인 패턴

`디자인 패턴` 이란 **전체 클래스 중 의미 있는 클래스들을 묶은 각각의 집합**을 생각하면 된다.

**반복되는 의미 있는 집합을 정의**하고, **이름을 지정 후 동일한 의미의 패턴으로써 다른 사람들과 공유할 수 있도록 만들어 놓는 것**이다.

*흔히 사용하는 `DAO` 역시 `Data Access Object 패턴` 으로 DB에 접근을 전담하는 클래스를 추상화하고 캡슐화 하는 패턴 중 하나이다.*

`Sun` 에서 제공했던 `Core J2EE 패턴` 중 성능과 밀접한 패턴은 `Service Locator 패턴` 과 `Transfer Object 패턴` 이 있다.

<br>

# Transfer Object 패턴

`Transfer Object 패턴` 은 **`Value Object` 라고 불리는 `Transfer Object` 데이터를 전송하기 위한 디자인 패턴**이다.

흔히 `Spring MVC 모델` 에서 `VO` 로 사용하는 아래와 같은 방식이다.

```jsx
public class GillogTO implements Serializable {
	private String name;
	private String id;

	public GillogTO() {
		super();
	}

	public GillogTO(String name, String id) {
		super();
		this.name = name;
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public String getID() {
		return id;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setID(String id) {
		this.id = id;
	}

	public String toString() {
		StringBuilder sb = new StringBuilder();
		sb.append("name=")
			.append(name)
			.append("id=")
			.append(id)
		return sb.toString();
	}
}
```


### Why Serializable implements????

**해당 인터페이스를 구현** 했다고 **반드시 구현해야 하는 메소드가 있는 것이 아니고, 변수가 존재 하는 것이 아니다.**

**다만 이 인터페이스를 구현하면 객체를 직렬화 할 수 있다.**

**서버 사이의 데이터 전송이 가능**해지므로 **원격지 서버에 데이터를 전송 하거나 파일로 객체를 저장할 경우**에는 **`Serializable Interface` 를 구현**해야한다.

<br>

## Tranfer Object 패턴 사용 목적

**`Transfer Object 패턴` 의 목적은 `Transfer Object` 를 만들어 하나의 객체를 통해 여러 타입의 값을 전달 하기 위함**이다.

필드 상의 **접근제한자를 private로 할지 public으로 할지에는 정답은 없지만, 성능 상으로 따져 볼 때는 getter(), setter() 메소드를 만들지 않는 것이 더 빠르다**.

하지만 **정보 은닉, 필드 값의 수정에 제한을 두기위해 getter(), setter() 메소드를 작성하는 경우가 일반적**이다.

그리고 **getter(), setter() 메소드를 사용**함으로써, **속성 값이 null이더라도 길이가 0인 String을 return**하여, **Transfer Object를 잘 정의 함으로써 각 소스에서 null 처리 과정을 편하게** 할 수 있다.

여기서 **toString()메소드의 구현은 권장**되는데, **이 메소드를 구현하지 않고 Gillog 객체의 toString() 메소드를 실행**하면, **`com.pattern.GillogTO@c1019` 와 같은 알 수 없는 값을 return**한다.

**`JUnit` 기반 테스트 코드 작성 시에 값 비교를 위해서나 데이터 확인을 위해 유용하게 사용** 되므로 **toString() 메소드를 반드시 구현하는 것이 좋다.**

 **`Transfer Object` 를 사용한다고 `Application` 에 엄청난 성능 개선 효과가 발생하지는 않는다.**

하지만 **한 객체에 결과 값을 담아 올 수 있기에, 두 세번씩 요청하는 일이 발생하는 것을 줄여주므로 해당 패턴 사용이 권장**된다.
