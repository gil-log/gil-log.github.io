---
title: "OAS(OpenAPI Specification)"
last_modified_at: 2021-10-07T17:35
categories: 
  - 개념
tags: 
  - 'OAS' 
  - 'Swagger' 
  - 'yaml'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[The Design of Web APIs](https://www.manning.com/books/the-design-of-web-apis)

---

# OAS(Open API Specification)

**`OAS(OpenAPI Specification)`**은 **REST 명세 포맷**으로 **널리 사용되고 있는 포맷 중 하나**이다.


**`OAS(OpenAPI Specification)`**는 **프로그래밍 언어에 종속되지 않고 사용**하는 **REST API 명세 포맷**이다.



**`OAS` 포맷**은 **`OAI(OpenAPI Initiative)`에서 만들어졌고**,

**[OAS(https://openapis.org)](https://openapis.org)에서 커뮤니티가 주도적으로 만들어가는 명세 포맷**이다.


---

## Swagger

**Swagger로 명세된 포맷**은 **2015년 11월 경 `OAI`로 기증**되어,

**2016년에 `OpenAPI Specification`으로 이름이 변경**되었다.


---
## API 명세 포맷??

**`API 명세`**는 **API 세부사항 표현을 위한 데이터 포맷**이다.

**아래 예시와 같은 텍스트 파일이 `API 명세`**이다.

```yaml
/gillog:
	description: developer
    get:
    	description: gillog
        content:
          application/json:
            schema:
              properties:
                date:
                  type: date
                  example: 작성일자
                name:
                  type: string
                  example: OAS
```

위와 같은 **설명 포맷을 통해**서,

**해당 API**의 **리소스 포함 관계**, **동작**, **RequestBody**, **Property** 등의 **정보를 정리된 형태**로 보여준다.

---



## API 명세 포맷 사용 이유


**`워드프로세서`, `스프레드시트` 등으로도 프로그래밍 인터페이스를 설명할 수 있고, 다른 이들과 공유도 용이**하다.

하지만 **버전관리**, **문서 자동 생성**, **문서 코드 생성**, **문서 바탕 API 연관 도구 설정 등 ...** 의 **단점이 존재**하고,


**API 명세 포맷 사용**으로 **디자인 단계를 포함해 API LifeCycle 전반적으로 이점**을 가질 수 있다.


### 1. 코드 작성하듯 API를 효율적으로 사용

**`OAS`문서**는 **단순 텍스트 파일 형태**로 **Git과 같은 버전 컨트롤 시스템에 쉽게 저장이 가능**하다.

이를 통해 **API 디자인을 반복하는 동안 간단하게 버전을 지정**하고, **수정사항을 추적할 수 있다.**

<br>

또한 **Resource, Action, Parameter, Response 등에 대해 작성**하고, **재사용 가능한 컴포넌트(데이터 모델) 정의를 통해**서,

**프로그래밍 인터페이스를 보다 효율적으로 설명**할 수 있게 해주고,

**위험하고 고통스러운 API 명세의 Copy & Paste 반복을 피할 수 있게 해준다.**


<br>

**`OAS` 문서를 작성할 때 사용하고 있는 텍스트 에디터 사용 보다**,

**`OAS` 포맷을 위해 만들어진 [Swagger Editor[http://editor.swagger.io/]](http://editor.swagger.io/)와 같은 온라인 에디터를 사용하는 것을 추천**한다.






### 2. API 명세 및 문서의 손쉬운 공유


**`OAS` 문서**는 **다른 팀이나 회사와 같은 타인**에게 **API 디자인에 대해 공유하고 피드백 받기 쉬운 수단**이다.

**내부 시스템에서 사용되는 특정 포맷**은 **소수만 이해**할 수 있지만,

**`OAS` 포맷으로 작성된 문서**는 **널리 통용되어 많은 사람들이 한번에 이해**할 수 있다.

또한 **`OAS`문서 양식에 익숙하지 않은 사람들을 위해**,

**`Swagger UI`와 같은 Tool**로 **`OAS`문서 사용을 위해 사용 가능한 모든 Resource와 Action을 보여주는 API 참조 문서를 생성할 수도 있다.**


### 3. 코드 생성 이상이 가능

**`API 명세 포맷`으로 API를 디자인** 했다면, 

**이 명세 포맷을 바탕**으로 **일부 구현 코드를 만들거나**,

**소스 코드 내용 없는 뼈대를 구성**하거나,

**동작하는 목업을 생성할 수도** 있다.

**기계도 읽을 수 있는 텍스트 파일 형태의 `OAS` 명세 포맷**을 통해 **API 소비 코드를 바로 생성하는 `API 명세 장점`을 취할 수도** 있다.

또한 **`API 명세 포맷` 사용**으로 **API나 보안 도구 테스트를 수행할 수도** 있으며,

이미 **개발된 다양한 API 관계 도구 Tool들도 사용할 수 있다.**
_예를 들어 **`API Gateway Solution`**은 **OAS 같은 API 명세 파일로 API 제공 프록시에 설정할 수 있다.**_




---

## YAML(YAML Ain't Markup Language)


**`OAS` 문서**는 **`YAML`이나 `JSON`으로 작성**할 수 있는데,

**읽고 쓰기 좋은 `YAML` 포맷으로 작성하는 것이 뜨는 추세**이다.


<br>

**`YAML(YAML Ain't Markup Language)`**은 **JSON처럼 사람이 이해하기 쉽게 쓰인 데이터 직렬화 포맷**이다.

**`YAML`**은 **Key, Value 형태의 데이터 포맷**으로 **아래와 같은 형식**이다.

```yaml
# YAML
simple-property: 1

object-property:
  a-property: a
  b-property: b
  
array-property:
  - item-1-property-1: one
  item-1-property-2: two
  - item-2-property-1: three
  item-2-property-2: four
  
# JSON
{
    "simple-property": "1"
    
    , "object-property": {
        "a-property": "a"
        , "b-property": "b"
    }
    
    ,  "array-of-objects": [
        {
          "item-1-property-1": "one"
          , "item-1-property-2": "two"
        }
        , {
            "item-2-property-1": "three"
            , "item-2-property-2": "four"
          }
    ]
}
```


### YAML 작성 주의사항

- **`"`는 속성과 값에서 허용되지 않는다.**

- **JSON에서는 `{}`, `,`로 구분**하지만, 
**YAML에서는 줄바꿈과 들여쓰기로 대체**된다.

- **JSON 배열에서 쓰던 `[]`, `,`**는,
**YAML에서는 `-`와 줄바꿈으로 대체**된다.

- **문장 시작에서 `#`을 통해 주석 처리가 가능**하다.





---