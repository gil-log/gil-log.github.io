---
title: "[Swagger UI] Annotation 설명"
last_modified_at: 2021-06-29T23:15
categories: 
  - spring
tags: 
  - 'Swagger' 
  - 'annotations'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Spring Boot에 Swagger 적용하기[기록하고 성장하기]](https://hyeran-story.tistory.com/73)

[Swagger UI 설정하기[상구리의 기술 블로그]](https://www.skyer9.pe.kr/wordpress/?p=1450)

[27. Spring - Swagger 기본사용법 및 API 문서자동화[kimjonghuyn]](https://kim-jong-hyun.tistory.com/49)

---

# Swagger

**`Swagger`**란 **서버로 요청되는 URL 리스트**를 **HTML화면으로 문서화 및 테스트 할 수 있는 라이브러리**이다.

간단하게 설명하면 `Swagger`는 `API Spec 문서`이다.

**API를 엑셀이나 가이드 문서를 통해 관리하는 방법**은 **주기적인 업데이트가 필요**하기 때문에 **관리가 쉽지 않고 시간이 오래** 걸린다.

그래서 **`Swagger`를 사용해 `API Spec 문서`를 자동화**해주어 **간편하게 API문서를 관리하면서 테스트**할 수 있다.

---

# Annotations

[Swagger 설정을 먼저 하고싶다면 여기](https://velog.io/@gillog/SpringBoot-Swagger-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)글을 먼저 읽고 오면 된다.

**Swagger에서 API 설명을 위한 Annotation 들을 아래에 정리**한다.

## @ApiOperation = Method 설명

`@ApiOperation`으로 해당 **Controller 안의 method의 설명을 추가**할 수 있다.


```java
    @ApiOperation(
        value = "사용자 정보 조회"
        , notes = "사용자의 ID를 통해 사용자의 정보를 조회한다.")
    @GetMapping("/user/{id}")
    @ResponseBody
    public UserDTO getUser(@PathVariable(name = "id") String id) {
        return userService.findUserInfoById(id);
    }
```

![](https://images.velog.io/images/gillog/post/5e665dc9-9d75-44d7-bed2-dafa13c6c3ac/image.png)


## @ApiImplicitParam = Request Parameter 설명

**`@ApiImplicitParam` Annotation**으로 **해당 API Method 호출에 필요한 Parameter들의 설명**을 추가할 수 있다.


```java
    @ApiOperation(
        value = "사용자 정보 조회"
        , notes = "사용자의 ID를 통해 사용자의 정보를 조회한다.")
    @ApiImplicitParam(
        name = "id"
        , value = "사용자 아이디"
        , required = true
        , dataType = "string"
        , paramType = "path"
        , defaultValue = "None")
    @GetMapping("/user/{id}")
    @ResponseBody
    public UserDTO getUser(@PathVariable(name = "id") String id) {
        return userService.findUserInfoById(id);
    }
```

![](https://images.velog.io/images/gillog/post/b8f30ea0-41fa-4c51-afe7-728b1d6958c5/image.png)


**`dataType`, `paramType`, `required`**의 경우 **해당 `name`의 정보가 자동으로 채워지므로 생략 할 수 있다.**

**`paramType`**의 경우 **`@RequestParam`은 `query`**를,

**`@PathVariable`은 `path`를 명시**해주면 된다.

만약 **해당 Method의 Parameter가 복수**일 경우,

**`@ApiImplictParams`로 `@ApiImplictParam`을 복수개 사용할 수 있다.**

```java
    @ApiOperation(
        value = "자격증 정보 조회"
        , notes = "자격증의 ID를 통해 자격증의 정보를 조회한다.")
    @ApiImplicitParams(
        {
            @ApiImplicitParam(
                name = "id"
                , value = "자격증 아이디"
                , required = true
                , dataType = "string"
                , paramType = "path"
                , defaultValue = "None"
            )
        ,
            @ApiImplicitParam(
                name = "fields"
                , value = "응답 필드 종류"
                , required = false
                , dataType = "string"
                , paramType = "query"
                , defaultValue = ""
            )
        })
    @GetMapping("/licenses/{id}")
    @ResponseBody
    public UserDTO getLicense(@PathVariable(name = "id") String id, @RequestParam(name = "fields", required = false) String fields) {
        return userService.findUserInfoById(id);
    }
```
![](https://images.velog.io/images/gillog/post/b931553a-7ae2-4883-b920-9cb85320edf7/image.png)


## @ApiResponse = Reponse 설명

**`@ApiResponse` Annotation**으로 **해당 method의 Response에 대한 설명**을 작성할 수 있다.

```java
    @ApiResponse(
        code = 200
        , message = "성공입니다."
    )
    @GetMapping("/notices/{id}")
    public String getNotice() {
        return "notice";
    }
```
![](https://images.velog.io/images/gillog/post/41743039-c170-4f49-8cac-1001a1a0c594/image.png)


**복수개의 Response에 대한 설명을 추가 하고 싶다면, 
`@ApiResponses`를 사용**하면 된다.

```java

    @ApiResponses({
        @ApiResponse(code = 200, message = "성공입니다.")
        , @ApiResponse(code = 400, message = "접근이 올바르지 않습니다.")
    })
    @GetMapping("/notices/{id}")
    public String getNotice() {
        return "notice";
    }
```

![](https://images.velog.io/images/gillog/post/c9697066-46eb-42c0-a633-d40b42f8f130/image.png)

## `Default Response Message` 삭제


![](https://images.velog.io/images/gillog/post/afb2a21b-aacc-404f-b460-a726cad94dce/image.png)

만약 위 사진에서 보이는 **`Default Response Message`들을 삭제하고 싶다면**,

**Swagger Config**에 **`Docket`에 `useDefaultResponseMessages(false)`를 설정**해주면 된다.

![](https://images.velog.io/images/gillog/post/464e88d6-9bd7-47de-bbba-8e02616e1858/image.png)

```java
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
                .consumes(getConsumeContentTypes())
                .produces(getProduceContentTypes())
                .useDefaultResponseMessages(false)
                .apiInfo(getApiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.bng.ddaja"))
                .paths(PathSelectors.ant("/**"))
                .build();
    }
```

**`useDefaultResponseMessages(false)`를 설정**해주면,

**`@ApiResponse`로 설명하지 않은`401`, `403`, `404` 응답들이 사라진다.**



## @ApiParam = DTO field 설명

**`@ApiParam` Annotation으로 DTO의 field에 대한 설명을 추가**할 수 있다.

```java
    @ApiResponses({
        @ApiResponse(code = 200, message = "성공입니다.")
        , @ApiResponse(code = 400, message = "접근이 올바르지 않습니다.")
    })
    @GetMapping("/notices/{id}")
    public String getNotice(UserDTO userDTO) {
        return "notice";
    }
    
    
@Getter
@Setter
@NoArgsConstructor
public class UserDTO {
    
    @ApiModelProperty(
        name = "id"
        , example = "gillog"
    )
    @ApiParam(value = "사용자 ID", required = true)
    private String id;

    @ApiParam(value = "사용자 이름")
    private String name;

    @ApiParam(value = "token 갱신 값")
    private String tokenRefresh;
}
```


![](https://images.velog.io/images/gillog/post/4f0aa633-e74c-45f1-8320-3536543bee06/image.png)


아래와 같이 **Controller에서 Parameter들에 바로 적용도 가능**하다.
_추천하지는 않는다._

```java
    @GetMapping("/debates/{id}")
    public String getDebate(@ApiParam(value = "토론 ID") @PathVariable(name = "id") String id) {
        return "debate";
    }
```

![](https://images.velog.io/images/gillog/post/2b72ee0d-21fa-47a1-bb61-6252284ab51a/image.png)


## @ApiModelProperty = DTO 예제 설명

**`@ApiModelProperty` Annotation**을 사용하는 **`DTO` Class Field에 추가**하면,

해당 **DTO Field의 예제 Data를 추가**할 수 있다.

```java
@Getter
@Setter
@NoArgsConstructor
public class UserDTO {
    
    @ApiModelProperty(
        name = "id"
        , example = "gillog"
    )
    @ApiParam(value = "사용자 ID", required = true)
    private String id;
    
    @ApiModelProperty(example = "길로그")
    @ApiParam(value = "사용자 이름")
    private String name;
}
```
여기서 **`name`은 생략**할 수 있다.

![](https://images.velog.io/images/gillog/post/30aeed1b-16b8-4ce6-95d9-5a6cf195ff7c/image.png)

## @ApiIgnore = Swagger UI 상 무시

```java
    @ApiResponses({
        @ApiResponse(code = 200, message = "성공입니다.")
        , @ApiResponse(code = 400, message = "접근이 올바르지 않습니다.")
    })
    @ApiImplicitParam(
        name = "id"
        , value = "사용자 아이디"
        , required = true
        , dataType = "string"
        , paramType = "path"
        , defaultValue = "None")
    @GetMapping("/notices/{id}")
    public String getNotice(UserDTO userDTO) {
        return "notice";
    }
```
![](https://images.velog.io/images/gillog/post/0ee553be-ea29-4eb8-b3f5-3d00e8a92450/image.png)


위 같은 코드에서 해당 method `getNotice`에서는 `UserDTO`의 `id` 만 필요한 상황인데,

**Parameter에 DTO를 삽입하여 모든 Parameter 정보가 노출**되었다.

**`@ApiIgnore`을 추가**함으로써, **`@ApiImplictParam`으로 선언하지않은 parameter 정보들을 무시**할 수 있다.

```java
    @ApiResponses({
        @ApiResponse(code = 200, message = "성공입니다.")
        , @ApiResponse(code = 400, message = "접근이 올바르지 않습니다.")
    })
    @ApiImplicitParam(
        name = "id"
        , value = "사용자 아이디"
        , required = true
        , dataType = "string"
        , paramType = "path"
        , defaultValue = "None")
    @GetMapping("/notices/{id}")
    public String getNotice(@ApiIgnore UserDTO userDTO) {
        return "notice";
    }
```
![](https://images.velog.io/images/gillog/post/c2366fca-09ca-4e1c-a6e0-e0e7d14ba0cf/image.png)

또한 **method의 `return type` 앞에 명시**해 **해당 method를 아예 Swagger UI 에서 노출되지 않게** 바꿀 수도 있다.

```java
    @ApiResponses({
        @ApiResponse(code = 200, message = "성공입니다.")
        , @ApiResponse(code = 400, message = "접근이 올바르지 않습니다.")
    })
    @ApiImplicitParam(
        name = "id"
        , value = "사용자 아이디"
        , required = true
        , dataType = "string"
        , paramType = "path"
        , defaultValue = "None")
    @GetMapping("/notices/{id}")
    public @ApiIgnore String getNotice(UserDTO userDTO) {
        return "notice";
    }
```

![](https://images.velog.io/images/gillog/post/53e5a8de-8ffa-416e-8ef6-bf92ed4597e9/image.png)

## Swagger UI API 화면 커스텀

**`Swagger Config` Class에서 `.apiInfo(ApiInfo Type)`을 작성**하면,

**Swagger UI 기본 화면의 API 설명 문구등을 커스텀**할 수 있다.
```java
@Configuration
@EnableSwagger2
public class SwaggerConfig{
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.OAS_30)
                .consumes(getConsumeContentTypes())
                .produces(getProduceContentTypes())
                .useDefaultResponseMessages(false)
                .apiInfo(getApiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.bng.ddaja"))
                .paths(PathSelectors.ant("/**"))
                .build();
    }

    private Set<String> getConsumeContentTypes() {
        Set<String> consumes = new HashSet<>();
        consumes.add("application/json;charset=UTF-8");
        consumes.add("application/x-www-form-urlencoded");
        return consumes;
    }

    private Set<String> getProduceContentTypes() {
        Set<String> produces = new HashSet<>();
        produces.add("application/json;charset=UTF-8");
        return produces;
    }

    private ApiInfo getApiInfo() {
        return new ApiInfoBuilder()
                .title("[DDaJa] REST API")
                .description("[DDaJa] BackEnd REST API Details")
                .contact(new Contact("[DDaja Swagger]", "https://github.com/swgil007/DDaJa", "BNG"))
                .version("1.0")
                .build();
    }
}
```

![](https://images.velog.io/images/gillog/post/5a2323e3-e48a-42c3-8c5b-6efb0f7eee11/image.png)


