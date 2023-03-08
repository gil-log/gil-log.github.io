---
title: "[Spring] Command Object"
last_modified_at: 2020-11-12T23:34
categories: 
  - spring
tags: 
  - '@RequestParam' 
  - 'Command Object' 
  - 'HttpServletRequest' 
  - 'Spring'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
`Command Object`에 대해 알아보기 전, Java에서 [Servlet](https://velog.io/@gillog/Servlet%EC%84%9C%EB%B8%94%EB%A6%BF)의 동작 과정을 다시 한번 상기해보고 `HttpServletRequest`와 `@RequestParam`을 이용한 data binding을 살펴보려 한다.
_[Servlet](https://velog.io/@gillog/Servlet%EC%84%9C%EB%B8%94%EB%A6%BF)??_


### Servlet Life Cycle

1. `Servlet Container`가 `Servlet Instance`의 init() method를 호출하여 초기화한다.
_최초 요청시 한번만 초기화되며 그 이후로는 이 과정을 생략_

2. `Servlet`이 초기화된 다음부터 Client의 Request를 처리할 수 있다. 
각 Request는 별도의 thread로 처리하고 이때 `Servlet`의 service() method를 호출한다.
_이 안에서 HTTP Request를 받고 클라이언트로 보낼 HTTP Response를 만든다._
_service() 는 Http Method 에 따라 doGet() 또는 doPost() 등으로 위임하여 처리한다._

3. `Servlet Container` 판단에 따라 `Servlet`을 메모리에서 내려야할 시점에 destroy() 를 호출한다.

<br>

`Servlet`의 동작 과정을 잠깐 상기해본 이유는 Spring에서 Controller와 View 사이에서 사용하는 `HttpServletRequeset`를 시작으로 `Command Object`에 대해 다루어 보기 위함이다.

<br>

---

# HttpServletRequest

**`HttpServletRequest`는 `Http Servlet`에 대한 요청 정보를 제공하도록 `ServletRequest`를 확장한 Interface**이다.

**`Servlet Container`는 `HttpServletRequest`를 생성하고, `Servlet`의 service() method의 매개변수로 보낸다.**



```java
public class HelloServlet extends HttpServlet {

  @Override
  public void init() throws ServletExcetion {
    System.out.println("init");
  }
  
  @Override
  public void doGet(HttpServletReqeust request, HttpServletResponse response) throws ServletExcetion {
    System.out.println("doGet");
  }
  
  @Override
  public void doPost(HttpServletReqeust request, HttpServletResponse response) throws ServletExcetion {
    System.out.println("doPost");
  }
  
  @Override
  public void destory() {
    System.out.println("destroy");
    
  }
}
```


**`HttpServletRequest`의 핵심 기능은 `Http Servlet`의 Request를 받아서 꺼내서 쓸 수 있다는 것**이다.


`HttpServletRequest request`로 Controller에서 View단의 데이터를 가져오는 과정을 살펴보면 아래와 같다.


```
<form action="<c:url value="/user/ins"/>" method="post"/>
  <input type="text" name="userName" value="Gillog" />
  <input type="text" name="phone" value="010-1234-5678" />
</form>
```
**`POST` 방식**의 경우 JSP 에서 form을 이용해 정보를 입력하고 Submit을 하면 formData형식 처럼 **key와 value 값으로 Body에 실려** **`HttpServletRequest`에 담겨서 Controller로 전달**된다.

**`GET` 방식**의 경우 **URL 뒤에 `/user/ins?userName=Gillog&phone=010–1234–5678` 형식으로 전달**되는데  **?key=value&key=value 형식으로 `HttpServletRequest`에 담아서 Controller로 전달**된다.
_물음표(?) 뒤의 문자열들을 `QueryString` 또는 `RequestParameter`라고 부른다._


그렇게 되면 아래와 같이 Controller에서 Data를 사용할 수 있다.
속성 하나당 변수 1개가 생성되는 1:1 방식이라 할 수 있다.

```java
@PostMapping("/ins")
public String ins(HttpServletRequest request) {
  String name = request.getParameter("name"); // key 값을 이용해서 꺼내올 수 있다.
  String phone = request.getParameter("phone"); // key 값은 input 에서 설정한 name 값이다.
  
  // 만약에 JSP 에서 설정한 name="userName" 이라는 키값이 여러개인 경우에는
  // getParameterValues() 메서드를 이용하여 배열로 받아올 수 있다.
  String[] names = request.getParameterValues("name");
  
  return REDIRECT_LIST;
}
```
---

# @RequestParam

**`@RequestParam`을 사용하는 방식도 `HttpServletRequest`와 동일**하게 1:1 방식이다. 
**차이점은 `HttpServletRequest` 대신 `@RequestParam` Annotation을 사용한다는 점**이다.

```java
@PostMapping("/ins")
public String ins(@RequestParam String userName, @RequestParam String phone) {
  // @RequestParam 뒤에 붙는 매개변수 변수명은 JSP 에서 설정한 name 의 key 값과 동일해야 한다.
  return REDIRECT_LIST;
}
```

`HttpServletRequest` 와 `@RequestParam` 을 이용하여 data를 받아오는 경우 아래와 같은 단점들이 있다.

1. `Request Parameter`가 많아질 수록 Controller 내부 Code나 매개변수가 증가한다.

2. 작성되는 Code 양이 많아지므로 Code 가독성이 떨어진다.

**이러한 문제를 해결하고자 나온것이 Spring에서 `Command Object(커맨드 객체)`**이다.

---

# Command Object
**`Command Object`**란 **`HttpServletRequest`를 통해 들어온 `RequestParameter`들을 setter method를 이용**하여 **Object에 정의되어있는 속성에 Binding이 되는 Object를 의미**한다.


**`Command Object`는 보통 VO나 DTO를 의미**하며, `HttpServletRequest` 로 받아오는**`RequestParameter`의 key 값과 동일한 이름의 속성들과 setter method를 가지고 있어야 한다.**

**Binding방식**은 **Spring이 내부적으로** **`HttpServletRequest` 와 `Command Object`의 setter method를 이용하여 자동 Binding**을 시켜준다.
_객체를 JSON 형식으로 변환하기 위해 Jackon2ObjectMapperBuilder가 autoDetectGettersSetters() method를 이용하는 것과 비슷_

```java
@Getter 
@Setter
public class User {
  private String userName;
  private String phone;
  private int age;
}
```

```java
@PostMapping("/ins")
public String ins(User user, Model model) {
  String name = user.getUserName();
  String phone = user.getPhone();
  int age = user.getAge();
  
  // user 파라미터를 model 에 담는다.
  model.addAttribute("user", user);
  return REDIRECT_LIST;
}
```

`Command Object`인 User Class를 사용함으로써 `HttpServletRequest`나 `@RequestParam`을 사용하는 것보다 훨씬 **Code 양도 줄고, 가독성도 좋아지고 간편**해졌다.



**@ModelAttribute Annotation을 사용하여 user parameter를 model에 담는 코드를 제거할 수도 있다.**

---

# @ModelAttribute

@ModelAttribute의 **사용 위치에따라 기능이 달라지는데, 크게 method 위에 사용되는 경우와 parameter 옆에 사용되는 경우로 나뉜다.**

@**ModelAttribute 의 기능 중 하나를 먼저 말하자면, `Command Object`와 같이 `Requeset Parameter`들을 Object Property에 Binding 시켜준다는 것**이다.

**@ModelAttribute를 생략해도 `Command Object`를 이용해서 Binding**이 되고, **@RequestParam 또한 생략해도 사실상 Binding이 가능**하다.

그 이유는** Spring 내부적으로 String 이나 int 등은 @RequestParam으로 보고, 그 외의 복잡한 객체들은 @ModelAttribute가 생략됬다고 간주하기 때문**이다. 

하지만 그렇다고 **무조건 생략하는 것은 위험**한데, Spring이 **간단한 숫자나 문자로 전달된 Request Parameter를 제법 복잡한 객체로 변환할 수도 있기 때문**이다.

@ModelAttribute 사용 위치별 기능들은 아래와 같다.

## Command Object 옆에 @ModelAttribute가 사용되는 경우

```java
@PostMapping("/ins")
public String ins(@ModelAttribute User user, Model model) {
  String name = user.getUserName();
  String phone = user.getPhone();
  int age = user.getAge();
  
  // user 객체를 모델에 담는 코드를 작성하지 않아도, 담겨져 있다.
  // 내부적으로 model.addAttribute("user", user); 로 담는다.
  // 만약 객체명과 변수명이 @ModelAttribute UserVo user 로 되어있는 경우에는 어떻게 담길까?
  // 클래스명을 기준으로 카멜케이스를 적용하여 model.addAttribute("userVo", user); 로 담는다.
  
  return REDIRECT_LIST;
}
```

**`@ModelAttribute`의 역할 중 하나는 model에 객체를 담아준다는 것**이다. 

`Command Object` 옆에 @ModelAttribute을 사용했을 때 얻는 또 다른 이점은 **@ModelAttribute 가 붙은 Parameter를 처리할 때는 @RequestParam 과 달리 검증(Validation)작업을 내부적으로 진행**한다.

**@RequestParam의 경우 스프링의 기본 타입 변환 기능을 이용해서 Request Parameter 값을 method Parameter Type으로 변환**하는데, 만약 숫자 타입의 파라미터라면 문자열 타입으로 들어온 요청 파라미터의 **타입 변환을 시도하고 실패하면 `Http 400 Bad Request` 응답**이 클라이언트로 가게 된다.

이 경우, 친절하게 메시지를 보여주고 싶으면 `org.springframework.beans.TypeMismatchException` 예외를 처리하는 예외 Resolver를 추가해주면 된다.

하지만** @ModelAttribute 의 경우 내부적으로 검증(Validation) 작업을 진행하기 때문**에 setter method를 이용하여 값을 **Binding하려고 시도하다가 예외를 만나게되면 작업이 중단되면서 `Http 400 Bad Request` 가 발생하지는 않는다. **

타입 변환에 실패해도 작업은 계속되며 BindingException Type의 Object에 담겨서 Controller로 전달된다.

보통 **등록이나, 수정을 처리하는 Handler method의 경우 다양한 검증을 실시해야 하고, 사용자의 입력 값에 오류가 있을 때에는 이에 대한 처리를 Controller에게 맡겨야 한다.**

따라서 **@ModelAttribute를 통해서 폼의 정보를 전달 받는 경우 Errors 객체나 BindingResult 객체를 @ModelAttribute가 붙은 Parameter 바로 뒤에 선언해서 검증 처리를 실시**한다.
_Errors 나 BindingResult 는 자신의 바로 앞에 있는 파라미터 검증에서 발생한 오류들만 전달해주기 때문에 @Valid 나 @Validated, @ModelAttribute 가 붙은 파라미터 바로 뒤에 선언되어야 한다._



## method 위에 @ModelAttribute가 사용되는 경우

Controller에서 **method 위에 @ModelAttribute 가 사용되는 경우**는, 해당 **Controller 내의 어떠한 Handler method들보다 먼저 동작**하게 된다.

```java
/**
 * @ModelAttribute 메서드가 먼저 동작하기 때문에,
 * 다른 핸들러 메서드에서 model 에 담겨져있는 user 키값을 이용하여 user 객체를 꺼내서 쓸 수 있다.
 */
@ModelAttribute("user")
public String initUser() {
  // 내부적으로 model.addAttribute("user", userService.findUser(FIRST_USER_SEQ)); 형태로 담는다.
  return userService.findUser(FIRST_USER_SEQ); 
}
```

따라서 **여러 Handler에서 공통으로 쓰이며**,** View 단에서도 꺼내 쓸 일이 있는 것들은 이런식으로 처리해서 사용**하기도 한다.


---

# Command Object 사용 예제

#### Command Object Class의 이름(카멜 표기)과 동일한 속성 이름을 사용해서 Command Object를 View에 전달한다.

```java
<!-- ContextPath/WEB-INF/view/register/step2.jsp -->
<!-- /register/step3 요청 경로로 컨트롤러에 요청하는 뷰 -->
<form action="step3" method="post">
    <input type="text" name="name" id="name">
    <input type="submit" value="제출">
</form>
<!-- 폼 값으로 tinkerbell 을 넣고 제출 -->
 
 
@RequestMapping(value = "/register/step3", method = RequestMethod.POST)
// RegisterRequest 커맨드 객체를 메소드의 파라미터로 사용
// 폼에서 넘어오는 값들이 RegisterRequest 커맨드 객체에 설정된다 
public String handleStep3(RegisterRequest regReq) {
 
    try{
        System.out.println(regReq.getName()); 
        // 출력 결과는 폼에서 넘어온 tinkerbell
 
        memberRegisterService.regist(regReq);
        return "register/step3";
    catch(AlreadyExistionMemberException ex){
        return "register/step2";
    }
}
 
 
// 커맨드 객체를 생성하는 클래스
class RegisterRequest{
 
    private String eamil;
    private String password;
    private String confirmPassword;
    private String name;
 
    ... setter, getter
}
 
 
<!-- ContextPath/WEB-INF/view/register/step3.jsp --> 
<!-- 뷰 에 전달되는 커맨드 객체 이름, registerRequest -->
<!-- 커맨드 객체에 담긴 값 사용 -->
${registerRequest.name}

```



#### 아래와 같이 뷰에서 `Command Object`에 접근할 때 사용할 속성명을 변경하려면, `@ModelAttribute Annotation`을 사용하면 된다.

```java
 
@RequestMapping(value = "/register/step3", method = RequestMethod.POST)
// @ModelAttribute 을 사용해서 모델에서 사용할 속성 이름을 formData 로 설정
// 커맨드 객체는 모델에 담길 때, formData 속성명을 가지고 담긴다
public String handelStep3(@ModelAttribute("formData") RegisterRequest regReq) {
    try {            
        memberRegisterService.regist(regReq);
        // 커맨드 객체 RegisterRequest 를 속성값 formData 로 지정해서 모델에 담고
        // register/step3 을 뷰에 담아서 리턴
        // viewResolver 에 의해 ContextPath/WEB-INF/view/register/step3.jsp 가 호출됨
        return "register/step3";
    } catch (AlreadyExistingMemberException ex) {
        return "register/step2";
    }
}

<!-- ContextPath/WEB-INF/view/register/step3.jsp --> 
<!-- 뷰 에 전달되는 커맨드 객체 이름, formData -->
<!-- 커맨드 객체에 담긴 값 사용 -->
${formaData.name}

 
```

#### 아래와 같이 포함관계에 있는 Class에 HTTP Request Parameter 이름이 "Propertyname.Propertyname" 과 같은 형식이면, 중첩 Property의 값을 처리한다.

```java

public class Respondent {
 
    private String name;
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
}
 
 
public class AnsweredData {
    
    // 포함관계를 맺는다
    private Respondent res;
 
    public Respondent getRes() {
        return res;
    }
 
    public void setRes(Respondent res) {
        this.res = res;
    }
}
 
/*
AnsweredData 클래스에서 res.setName("냥냥"); 으로 
Respondent 클래스의 필드값을 설정해 줄 수 있다(중첩 프로퍼티).
이런 포함관계에 있는 클래스가 있을 때, 
뷰 JSP 에서 AnsweredData 커맨드 객체를 통해 Respondenet 객체의 값을 설정하려면,
<input type="text" name="res.name"> 처럼
<input> 태그의 name 속성을 설정해 주면 된다.
그러면 스프링은
commandObj.getRes().setName(request.getParameter("res.name"));
과 유사한 방식으로 커맨드 객체에 파라미터 값을 전달한다.
*/
 
// 컨트롤러
@Controller
@RequestMapping("/survey")
public class SurveyController {
 
    // 주소창에 직접 survey 경로를 입력하면 이 메소드가 실행
    // 리다이렉트 방식이 아니므로 주소 변동 없음, 그대로 요청한 경로 그대로 survey
    // 이 메소드가 리턴하는 뷰 surveyForm.jsp 에서 POST 방식으로 요청을 보내면
    // 주소에 변동이 없었기 때문에, POST 방식으로 /survey 요청 
    @RequestMapping(method = RequestMethod.GET)
    public String form() {
        return "survey/surveyForm";
    }
 
    // survey/surveyForm.jsp 에서 POST 방식으로 /survey 요청이 들어오면 이 메소드가 실행
    @RequestMapping(method = RequestMethod.POST)
    public String submit(@ModelAttribute("ansData") AnsweredData data) {
        return "survey/submitted";
    }
}
 
 
<!-- survey/surveyForm.jsp -->
<!-- POST 방식으로 /survey 요청을 하는 뷰 -->
<!-- AnsweredData와 포함관계에 있는 Respondent 객체의 name 필드에 값이 들어간다 --> 
<input type="text" name="res.name">
 
 
<!-- ansDate 속성명을 가지는 AnsweredData 커맨드 객체의 값 사용 --> 
<!-- survey/submitted.jsp -->
<!-- survey/surveyForm.jsp 에서 POST 방식으로 /survey 요청을 했을 때 결과를 보여주는 뷰 -->
 
${ansData.res.name}
<!-- 커맨드객체속성명.프로퍼티이름.프로퍼티이름 -->
<!-- AnsweredData 커맨드 객체와 포함관계에 있는 Respondent 객체의 name 필드 값이 출력 -->
```

#### 아래와 같이 Collection Property의 경우 HTTP Request Parameter 이름이 "Property[index]" 형식이면 List 타입의 Property의 값 목록으로 처리한다.

```java
public class AnsweredData {
    
    private List<String> responses;
    private Respondent res;
 
    public List<String> getResponses() {
        return responses;
    }
 
    public void setResponses(List<String> responses) {
        this.responses = responses;
    }
}
 
/*
뷰 JSP 에서 AnsweredData 커맨드 객체의 
이름이 responses 이고 List 타입인 컬렉션에 값을 담으려면,
요청 파라미터 이름으로 responses 를 사용하고,
인덱스 값을 [] 를 이용해서 지정해 주면 된다.
<input type="text" name="responses[0]"> => List 컬레션의 첫 번째 인덱스에 들어가는 값
<input type="text" name="responses[1]"> => List 컬레션의 두 번째 인덱스에 들어가는 값
폼 입력으로 responses[0] 에 냥냥, responses[1] 에 멍멍 을 입력하고 전송
==> List 컬렉션에는 {"냥냥", "멍멍"} 이 담기게 된다.  
*/
 
// 컨트롤러
@Controller
@RequestMapping("/survey")
public class SurveyController {
     
    @RequestMapping(method = RequestMethod.GET)
    public String form() {
        return "survey/surveyForm";
    } 
    
    @RequestMapping(method = RequestMethod.POST)
    public String submit(@ModelAttribute("ansData") AnsweredData data) {
        return "survey/submitted";
    }
}
 
 
<!-- survey/surveyForm.jsp -->
<!-- POST 방식으로 /survey 요청을 하는 뷰 -->
 
<input type="text" name="responses[0]">
<input type="text" name="responses[1]">
 
 
<!-- ansDate 속성명을 가지는 AnsweredData 커맨드 객체의 값 사용 -->
<!-- survey/submitted.jsp --%>
<!-- survey/surveyForm.jsp 에서 POST 방식으로 /survey 요청을 했을 때 결과를 보여주는 뷰 -->
 
<c:forEach var="response" items="${ansData.responses}" varStatus="status">
    ${status.index + 1}번: ${response}</li>
</c:forEach>
 
<!-- froEach JSTL 을 사용해서 AnsweredData 객체의 
resoponses 이름을 가지는 List 컬렉션에 담긴 값을 하나씩 꺼내와서 출력 --> 

```


<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[@ModelAttribute 와 커맨드 객체(Command Object)[Dope]](https://medium.com/webeveloper/modelattribute-%EC%99%80-%EC%BB%A4%EB%A7%A8%EB%93%9C-%EA%B0%9D%EC%B2%B4-command-object-42c14f268324)

[Spring MVC (커맨드 객체, 모델)[Welcome to NeverLand]](https://tinkerbellbass.tistory.com/42)

[]()

[]()

[]()

[]()