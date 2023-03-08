---
title: "[Spring] Parameter Mapping(MultiValueMap, @RequestParam, @RequestBody, @ModelAttribute, @SessionAttributes)"
last_modified_at: 2021-05-10T11:58
categories: 
  - spring
tags: 
  - '@RequestParam' 
  - '@SessionAttributes' 
  - 'MultiValueMap' 
  - 'RequestBody' 
  - 'Spring' 
  - 'modelattribute' 
  - 'parameter'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[파라메터(parameter) 어노테이션 및 객체 정보 및 설명[하늘을향해]](https://jbluewing.tistory.com/entry/%ED%8C%8C%EB%9D%BC%EB%A9%94%ED%84%B0parameter-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-%EB%B0%8F-%EA%B0%9D%EC%B2%B4-%EC%A0%95%EB%B3%B4-%EB%B0%8F-%EC%84%A4%EB%AA%85)

[[Spring] @RequestBody, @ModelAttribute, @RequestParam의 차이[MangKyu's Diary]](https://mangkyu.tistory.com/72)

[쿼리 파라미터(@RequestParam)값 얻기[devkuma]](http://www.devkuma.com/books/pages/471)

[]()

[]()

[]()

<br>

**`Spring`에서 `Controller`**에 원하는 **`Parameter`나 `Data`를 `Binding` 시키는 방법은 여러 종류**가 있다.

평소 개발할때 **사용하던 코드에서 아무 생각없이 Binding**을 하다보니 아래 **해당 Binding 방법들에 대한 이해도가 부족**하여 **`@RequestParam`, `@RequestBody`, `@ModelAttribute`, `MultiValueMap`에 대해 한번 알아보려한다.**

<br>

---

# @RequestParam



**`@ReqeustParam`**은 **해당 Request에 대한 Parameter들을 자동으로 `Binding`**한다.

**1개의 HTTP Request Parameter를 받기 위해서 주로 사용**하며 **`@RequestParam`은 필수 여부가 true**이기 때문에 **기본적으로 반드시 해당 Parameter가 전송되어야 한다.** 
_**해당 Parameter가 전송되지 않으면** **400 Error 발생**_ 

**그래서 `required`의 값을 false로 설정해 필수 여부를 조절**할 수 있고, **해당 Parameter를 사용하지 않고 요청을 보낼 경우**에 **`default`로 받을 값을 `defaultValue` 옵션을 통해 설정**할 수도 있다.


**주의사항**으로는 **단일 값만 Binding가능**하여 **만약 List형태로 복수의 Paramter를 Request**하면 **첫번째 Data 값만 Binding**된다.

**아래와 같은 형태로 많이 사용**된다.

```java
@ReqeustParam HashMap<String,Object> params

//{id=gil, name=gillog}
```

### MultiValueMap

**`MultiValueMap`**은 **`Spring`에서 제공하는 `Data Collection`**으로 **List형태의 값들**을 **Value로 Binding할 수 있는 특징**을 지닌다.

**하나의 Parameter에 여러 Data를 전송받아야 할 때 주로 사용**한다.



```java
@ReqeustParam MultiValueMap<String, String> multiParams


//{id=[gil], name=[gillog, loadbear]}
```


---

# @RequestBody

**`@RequestBody`**는 **Client가 전송하는 `Json(application/json)` 형태의 `HTTP Body` 내용**을 **Java Object로 Binding시켜주는 역할**을 한다. 
_**`HTTP Body`가 존재하지 않는 `Get` method**에 **`@RequestBody`를 사용시 에러가 발생**하게 된다._


**`@RequestBody`로 받는 `Data`**는 **`Spring`에서 관리하는 `MessageConverter`들 중 하나**인 **`MappingJackson2HttpMessageConverte`를 통해 Java Object로 Binding**된다.

**`@RequestBody`는 Setter함수가 없어도 요청받은 데이터를 Binding할 수 있다.**


---

# @ModelAttribute


** `@ModelAttribute`** 는 **Client가 전송하는 `multipart/form-data` 형태의 `HTTP Body` 내용과 Parameter들**을 **`Setter`를 통해 1:1로 Java Object에 Binding하기 위해 사용**된다. 


**`@ModelAttribute`에는 Mapping 시키는 Parameter의 타입이 Object의 타입과 일치하는지를 포함**한 **다양한 검증(Validiation) 작업이 추가적으로 진행**된다.
_**int형 index 변수**에 **"1번" 이라는 String형을 Binding**시키려 하면, **`BindException`이 발생**하게 된다._


### VS @RequestBody

**`@RequestBody`는 `Json`이나 `XML`과 같은 형태의 데이터를 `MessageConverter`를 통해 Binding하고,**
**`@ModelAttribute`는 `multipart/form-data` 형태**의 **`HTTP Body`와 `HTTP Parameter`들을 Mapping시킨다.**

**`@ModelAttribute`는 Setter함수가 없다면 Binding이 되지 않는다.**

<br>

### 특정 Paramter Binding

아래 예제 코드 처럼 **특정 Parameter만 Binding 할 수도 있다.**

```java

@ModelAttribute('writer') String writer

//{ writer: 'gillog', contents : 'ModelAttributeStudy' }
```

### JSP Forward

`@ModelAttribute`는 **`@RequestParam` Annotattion과 달리** **`JSP`로 `Forward`**했을 때 **`Request`로 받은 값을 별다른 Mapping 과정 없이 그대로 전달**할 수 있다.


**만약 아래 코드처럼 `@RequestParam` Annotation을 사용**한다면,

```java
@GetMapping(value = "/requestMapping")
public String requestParamMapping(@RequestParam HashMap<String, Object> params, Model model) {
	model.addAttribute("requestMapping", params);
	return "views/jsp/gillog";
}
```
**`Model`을 이용, `JSP`로 `Forward`할 때 Binding 과정이 추가** 된다.

<br>

하지만 **`@ModelAttribute` Annotation을 사용**하면 **Binding 과정 없이** 바로 **`JSP`로 Parameter로 받은 Data를 `Forward`할 수 있다.**


```java
@GetMapping(value = "/requestMapping")
public String requestParamMapping(@ModelAttribute HashMap<String, Object> params) {
	return "views/jsp/gillog";
}
```

---

# @SessionAttribute

**`@SessionAttributes`**는 **`Controller` Class에 지정되는 Annotation**으로 **`@ModelAttribute` Annotation과 동일한 특징**에, **해당 값을 `Session`에도 저장**해 준다.

이때 **`@SessionAttributes` Annotation에 정의한 Key**와, **동일한 Key로 Model에 값을 Binding 되는 경우에 해당**하고,
**해당 Class, `Controller` 내에서만 `@SessionAttributes` Annotation으로 지정한 Session 값을 사용**할 수 있다.

**같은 `Controller` 안에서 다루는 특정 `Model` Data를 Session에 넣고 공유하는 용도로 사용**된다. 
 
 
```java
@Controller
@SessionAttributes("gillog")
public class GillogController {
	@GetMapping(value = "/requestMapping")
	public String requestParamMapping(@RequestParam HashMap<String, Object> params, Model model) {
		model.addAttribute("requestMapping", params);
		return "views/jsp/gillog";
	}

	@GetMapping(value = "/requestMapping")
	public String requestParamMapping(@ModelAttribute HashMap<String, Object> params) {
		return "views/jsp/gillog";
	}
}
```
