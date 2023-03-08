---
title: "[DB]Domain, Data Dictionary"
last_modified_at: 2020-10-28T06:40
categories: 
  - database
tags: 
  - 'Data Dictionary' 
  - 'db' 
  - 'domain' 
  - '개념'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# Domain(도메인)

**`Domain` **이란 **엔티티의 속성들이 가질 수 있는 값들의 집합**으로, 관계형 이론에서 도메인은 실제로는 구현이 어렵기 때문에, 대부분의 DBMS에서 **도메인이란 속성에 대응하는 컬럼에 대한 데이터 타입(Data Type)과 길이를 의미**한다.

**두 속성의 도메인이 같다**라는 것은 **두 속성의 데이터 타입과 길이가 같다는 것을 의미**한다.

**정의된 도메인명은 일반적인 데이터 타입처럼 사용**할 수 있다.


<br>


`Domain`은 **데이터 모델링 관점에 있어 데이터 표준화 측면에서 매우 중요한 요소**다.

도메인이 체계화 및 표준화되지 않으면 물리 모델 구현 관점에서 **속성과 엔티티의 이름이 표준화되지 않을 수 있고 데이터 타입의 정확성까지 잃을 수 있기 때문**이다. 
_개발 부서들 마다 테이블을 생성하다 서로 외래키로 참조해야하는 상황에서 컬럼 데이터 타입의 불일치 상황 등_


컬럼에 데이터 타입을 작성하는 방법은 다음과 같다.


- **직접 데이터 타입을 지정**

일반적으로 테이블 생성 시 사용하는 방법으로 DDL 작성시에 테이블 생성 때 컬럼의 데이터 타입을 지정해주는 방법이다.

```
CREATE TABLE EMP (
empid	   int(7),
emp_name   varchar(20)
manager	   int(7)
address    varchar(100)
SEX	   char(1)
);
```

- **도메인을 이용한 컬럼 데이터 타입 지정**


아래와 같이 도메인을 생성하여 성별을 'M', 'F' 와 같이 정해진 1개의 문자로 표현하여 컬럼의 데이터 타입을 설정 할 수도 있다.

```
CREATE DOMAIN SEX CHAR(1)
       DEFAULT 'M'
       CONSTRAINT VALD-SEX CHECK (VALUE IN('M'.'F'));
       
       
CREATE TABLE EMP (
empid	   int(7),
emp_name   varchar(20),
manager	   int(7),
address    varchar(100),
SEX	   SEX
);       
       
```

다음은 도메인 생성 시에 사용되는 문법이다.


```
CREATE DOMAIN 도메인명 데이터_타입
       [DEFAULT 기본값]
       [CONSTRAINT 제약조건명 CHECK (범위 값)];
```
- 데이터 타입 : SQL에서 지원하는 데이터 타입

- 기본 값 데이터를 입력하지 않았을 때 자동으로 입력되는 값



# Domain 정의 방법

도메인은 다음과 같은 절차를 통해 정의 할 수 있다.


1. 모든 속성을 엔티티별로 나열한다.


![](https://images.velog.io/images/gillog/post/98ebd8fe-94bb-44e7-b550-899aa3691284/1.png)



2. 속성 이름에 복합 명사들을 분리하여 명사 별로 정리한다.


![](https://images.velog.io/images/gillog/post/82e5d3ff-bf2d-46d6-a654-a4d4e593cadc/2.png)


3. 마지막 명사를 기준으로 정렬한다.


![](https://images.velog.io/images/gillog/post/56ef77bc-516a-4406-8966-84cebed76358/3.png)



4. 도메인 명을 정해 준다.



![](https://images.velog.io/images/gillog/post/d0839e66-4293-42ba-bc28-0d5d1de1922f/4.png)




5. 도메인만 모아 중복을 제거한 후 데이터 타입과 길이, 초기 값, 범위 등을 정해준다.



![](https://images.velog.io/images/gillog/post/b8187f91-6102-4818-97b1-12b8e96ad606/5.png)



# Data Dictionary

**`Data Dictionary`**란 용어 사전이란 의미로, 논리적, 물리적 데이터베이스 설계시에 사용되는 용어들의 의미를 정의해 놓은 문서를 의미한다.

여기서 용어란 테이블(엔티티)와 컬럼(속성)의 이름을 뜻한다.

**`Data Dictionary`**는 테이블과 컬럼의 의미를 설명해 놓은 사전으로 **동일한 의미의 용어를 설계자들이 서로 다르게 사용하면서 생기는 혼란을 방지**해 준다.

논리 설계 단계에서 한글로 작성한 테이블, 컬럼 이름을 물리 설계 단계에서 영어 이름으로 바꿀 때 **통일성을 위하여 사용**한다.



# Data Dictionary 정의 방법

앞서 살펴본 도메인 정의 2단계 까지는 동일하게 진행된다.


모든 속성을 엔티티별로 나열한 후 속성 이름에 복합 명사들을 분리하여 명사 별로 정리한다.


그다음 분리된 단어들을 모두 모아 정렬해주고, 각 단어에 대해 영어 단어를 붙이고 용어의 의미를 설명해준다.
![](https://images.velog.io/images/gillog/post/8585d359-2f74-49ec-a11f-4d7ebba9a4e5/a1.png)

명명규칙은 여러 단어로 구성된 용어의 정의에 적용되며 아래와 같이 각 단어에 영어 단어를 붙여준 후, 자주 사용되는데 긴 영어 단어는 약어를 정의해주고 `_`를 이용해 사용한다.

[EX]이메일 수신 거부 > email receive deny > email_receive_deny > email_rcv_deny

![](https://images.velog.io/images/gillog/post/9b383ae8-0f2e-4c0a-9f28-0259b7f51069/a2.png)

논리 명(한글)에 대한 물리명(영어)를 작성해주고 약어를 입력해 준후 설명을 입력해 주면 용어사전이 작성되었다.
![](https://images.velog.io/images/gillog/post/b3155663-3f77-4646-9fb3-4cd664e07017/a3.png)

최종적으로 도메인과 용어 사전을 이용해 완성된 도메인 정의서 모습이다.

![](https://images.velog.io/images/gillog/post/38fbf3b9-28c5-4dc7-bccb-bb15107d30c1/a4.png)





<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[도메인의 개념을 이해하자[권순용의 데이터모델링 이야기]](http://www.gurubee.net/lecture/2872)

[데이터베이스 DDL[세상의 모든 기록]](https://all-record.tistory.com/61)

[데이터베이스 ch9 도메인과 용어사전의 정의](http://blog.naver.com/PostView.nhn?blogId=kimbh666&logNo=221391961062&parentCategoryNo=&categoryNo=18&viewDate=&isShowPopularPosts=true&from=search)

[]()

[]()

[]()