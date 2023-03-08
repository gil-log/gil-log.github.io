---
title: "[DDaJa] Swagger로 REST API 문서 초안 작성"
last_modified_at: 2021-06-30T23:12
categories: 
  - sideproject
tags: 
  - 'DDaJa' 
  - 'REST API' 
  - 'Side Project' 
  - 'Springboot' 
  - 'Swagger'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
오늘은 Side Project인 DDaJa의 **Spring Boot API Server**에,

지난번에 `B`와 함께 작성한 **API 설계 URI를 토대로 API 문서를 Swagger를 통해 생성**해보려한다.


![](https://images.velog.io/images/gillog/post/3f56500e-b59b-4608-8ba0-1a42c49f35bb/image.png)

![](https://images.velog.io/images/gillog/post/6dfa3322-9c58-4274-9404-73c510a542c4/image.png)


우리 Project에서 **필요한 `Resource`에서 `Collection`을 구분** 짓고

**계층 구조를 URI로 표현**하고, **`HTTP Method` 당 수행 역할을 간략하게 정리**해보았다.


이제 **[Swagger Annotation](https://velog.io/@gillog/Swagger-UI-Annotation-%EC%84%A4%EB%AA%85)을 활용해서 Spring Boot 상에서 API 문서를 작성**한다.


# Project Structure 생성


우리 Project에서는 **`Collection`들을 폴더 구조로 계층 관계로 표현**하고,

그 안에 **각 `Collection`당 `Controller`, `Service`, `DTO`, `Repository`의 구조**로 진행한다.

**아래와 같은 구조**를 가진다.


![](https://images.velog.io/images/gillog/post/8112e837-2fdc-40ce-ab7c-23b3d6d1f008/image.png)

먼저 **최상위 `Collection` 중 하나인 `licenses(자격증)`**은 **하위 `Collection`인 `mock-exams(모의고사)`, `subjects(과목)`, `words`를 가지고** 자신만의 **`controller`, `service`, `repository`, `dto` 폴더**를 가진다.

그리고 하위 **`mock-exams(모의고사)`는 또 하위 `Collection`으로 `subjets`를** 가지고, **자신만의 `controller`, `service`, `repository`, `dto` 폴더**를 가진다.

# Swagger 작성

이제 위와 같은 Project Structure에서 이제 Swagger Annotation으로 API 문서를 작성한다.

```java
@Controller
@RequestMapping("licenses")
@AllArgsConstructor
public class LicensesController {
    
    @ApiOperation(
        value = "자격증 전체 조회"
        , notes = "전체 자격증 목록을 조회한다")
    @GetMapping("")
    @ResponseBody
    public String getLicenses() {
        return "getLicenses";
    }
    @ApiOperation(
        value = "특정 자격증 조회"
        , notes = "자격증 시퀀스를 통해 특정 자격증의 정보를 조회한다.")
    @ApiImplicitParams({
        @ApiImplicitParam(
            name = "seq"
            , value = "자격증 시퀀스"
            , defaultValue = ""
        )
        , @ApiImplicitParam(
            name = "fields"
            , value = "요청 데이터 필드 값"
            , defaultValue = ""
        )
        , @ApiImplicitParam(
            name = "page"
            , value = "요청 데이터 페이지"
        )
        , @ApiImplicitParam(
            name = "perPage"
            , value = "요청 데이터 페이지 당 데이터 수"
        )
    })
    @GetMapping("/{seq}")
    @ResponseBody
    public String getLicenses(
        @PathVariable(name = "seq", required = true) long seq
        , @RequestParam(name = "fields", required = false) String fields
        , @RequestParam(name = "page", required = false) int page
        , @RequestParam(name = "per-page", required = false) int perPage) {
        return "getLicenses";
    }

}

```
위 코드와 같이 작성하면,

![](https://images.velog.io/images/gillog/post/4905024c-b4dd-4468-9a8e-64facb1e320d/image.png)


Swagger 상에서 해당 API를 자동으로 문서화 해준다.

