---
title: "[Swagger] @ApiResponse response = Response.Class  설정해도 응답 Class 적용 안되는 에러 해결"
last_modified_at: 2021-07-01T23:56
categories: 
  - 에러
tags: 
  - '@ApiResponse' 
  - 'OAS_30' 
  - 'SWAGER_2' 
  - 'Swagger' 
  - 'error'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[bleepcoder[Springfox: @ApiResponse response parameter not used in 3.0.0]](https://bleepcoder.com/springfox/680455328/apiresponse-response-parameter-not-used-in-3-0-0)
# 문제 상황

**`Swagger`를 사용해서 API 문서 생성 중**,

**`@ApiResponse` Annotation**으로 **응답 Code 별 Response를 따로 DTO Class로 관리**하고 싶던 와중,

아래와 같이 작성해도 **응답 결과가 빈값으로 출력**되고 있었다.

![](https://images.velog.io/images/gillog/post/4c7c493c-ae24-41cf-b9ab-3a5c48d15c50/image.png)


```java

    @PostMapping("")
    @ApiOperation(
        value = "신규 자격증 생성"
        , notes = "신규 자격증을 생성한다."
        , produces = "application/json"
        , response = ResponseDTO.class
    )
    @ApiImplicitParams({
        @ApiImplicitParam(
            name = "requestDTO"
            , value = "생성을 위해 필요한 Requeest Body"
        )
    })
    @ApiResponses({
        @ApiResponse(
            code = 200
            , message = "생성 성공"
        )
        , @ApiResponse(
            code = 201
            , message = "생성된 자원 정보"
            , response = ResponseDTO.class
            , responseContainer = "List"
        )
        , @ApiResponse(
            code = 409
            , message = "로직 수행 불가 모순 발생"
            , response = ErrorDTO.class
            , responseContainer = "List"
        )
    })
    @ResponseBody
    public ResponseDTO postLicenses(@RequestBody RequestDTO requestDTO) {
        return new ResponseDTO();
    }
```    
    
```java    
@Getter
@Setter
@ToString
@NoArgsConstructor
@ApiModel(description = "에러 상태")
public class ErrorDTO {
    
    @ApiModelProperty(example = "1")
    private String code;
    @ApiModelProperty(example = "올바르지 않은 회원 ID")
    private String message;
}
```

```java


@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class ResponseDTO {
    
    @ApiModelProperty(example = "111")
    private long seq;

    @ApiModelProperty(example = "길로그")
    private String name;
    
    @ApiModelProperty(example = "gillog")
    private String id;

    @ApiModelProperty(example = "2021-07-02")
    private Date created;

    @ApiModelProperty(example = "[{'rel' : 'self', 'href' : 'https://api.ddaja.com/licenses/111', 'method' : 'GET'}]")
    private List<String> links;
}

```

# 해결 과정

나는 **`Swagger`, `@ApiResponse`, `NotWorking` 등으로 Google 신의 도움**을 구했고,

**해당 이슈가 Swagger 3.0.0대 Model과 관련있다는 걸 발견**했다.

![](https://images.velog.io/images/gillog/post/300b4819-14ea-4b53-b35b-2f30a1197610/image.png)

**Spring fox 3.0은 v3 Model을 사용**하는데,

**source.getResponses() 함수의 `Type MissMatch` 관련 이슈** 였다.


# 해결 방법

해결 방법으로는 크게 두 가지가 있는데,

**Swagger v3 model 설정을 끄는 방법**과,

**`Docket` 객체**의 **DocumentationType을 2버전 대로 내리는 방법**이 있다.

## spring boot 설정에서 swagger의 `use-model-v3` 를 `false`로 지정

첫 번째 방법은 **spring boot 설정(application.properties)에서 swagger의 `use-model-v3` 를 `false`로 지정하는 방법**이다.


```
springfox.documentation.swagger.use-model-v3=false
```

![](https://images.velog.io/images/gillog/post/a2f9036d-c07d-4ea3-addd-f12891f288d8/image.png)


하지만 나는 위 방법으로 해결하지 못했다.


## DocumentationType.OAS_30 > DocumentationType.SWAGGER_2

두 번째 방법은 **`SwaggerConfig`**에서 **`Docket` 객체 생성 Prameter의 `DocumentationType`을 `SWAGGER_2`로 지정하는 방법**이다.
![](https://images.velog.io/images/gillog/post/99083265-04cd-49fc-9ca8-594a07bbfdf2/image.png)



![](https://images.velog.io/images/gillog/post/ffff5d31-1aa6-4857-9fd8-a2912cad240e/image.png)

```java
new Docket(DocumentationType.SWAGGER_2)
```

![](https://images.velog.io/images/gillog/post/d390f13d-2b6b-45fd-a8ff-7018bfb0fb50/image.png)

**위와 같이 변경하여 결국 해결**할 수 있었다.

<br>

**오늘도 해결 완료 🙆🏻‍♂️**