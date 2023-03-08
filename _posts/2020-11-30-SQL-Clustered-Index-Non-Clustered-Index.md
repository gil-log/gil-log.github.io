---
title: "[SQL] Clustered Index & Non-Clustered Index"
last_modified_at: 2020-11-30T23:41
categories: 
  - database
tags: 
  - 'Clustered Index' 
  - 'Index' 
  - 'NonClustered Index' 
  - 'sql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
앞서 [Index](https://velog.io/@gillog/SQL-Index%EC%9D%B8%EB%8D%B1%EC%8A%A4)에 대해서 정리 했었지만, `Clustered Index`와 `Non-Clustered Index`를 자세히 다루지 않아 이번 시간에 자세히 두 Index의 차이를 정리해보려 한다.


# Index 간략 정리

**`Index`는 DB의 테이블에 데이터가 많을 때, 검색 속도를 향상시켜주기위해 사용**된다. 

일반적으로 **`Index`에 비유되는 예가 책의 `색인` 혹은 `목차`**이다.



**책에 색인이 없다면** '길로그'라는 단어가 몇 페이지에 있는지 찾기 위해 책의 **첫 페이지부터 차례대로 찾아야** 하고, **최악의 경우 마지막 페이지에 도달**해서야 '길로그'라는 단어를 찾게 될 것이다. 

하지만 **`색인`이 있다면, 한번에 '길로그'가 있는 곳으로 찾아 갈 수 있다. **


**이를 해결하기 위해** 책에서는 `색인`, **DB에서는 `Index`를 사용**한다.


## Index 사용 특징

**`Index`를 사용하면 검색 속도의 향상 효과**를 볼 수 있다.
_**시스템 부하를 줄여, 시스템 전체 성능향상에 기여** 가능_



하지만 **`Index`를 위한 추가 공간이 필요**하고, **데이터가 많이 있다면 생성에 많은 시간이 소요** 될 수 있다.

**`INSERT`, `UPDATE`, `DELETE`가 자주 발생한다면 성능이 많이 하락**할 수 있다.


## Index 생성

**`Index`는 열 단위로 생성**되는데, **하나의 열에 `Index`를 생성**할 수 있고, **여러 열에 하나의 `Index`를 생성 할 수도** 있다.

**테이블 생성시 하나의 열에 `Primary Key`를 지정**하면 **자동으로 `Clustered Index`가 생성**된다.
_Clustered Index가 없는 경우_

**`UNIQUE` 제약 조건이 있는 테이블**을 만들면 데이터베이스 엔진에서는 **자동으로 `Non-Clustered Index`를 만든다.**

**`Primary Key`를 지정하는 열에 강제적으로 `Non-Clustered Index`를 지정 가능**하다.

**기존 테이블에 `PRIMARY KEY` 제약 조건을 적용**하려 하거나 **해당 테이블에 `Clustered Index`가 이미 있으면 `Non-Clustered Index`를 사용하여 기본 키를 적용**한다.

**제약 조건 없이 테이블 생성시에 `Index`를 만들 수 없으며**, **`Index`가 자동 생성되기 위한 열의 제약 조건은 `Primary Key`또는 `Unique` 뿐**이다.


 



<br>


이러한 **`Index`에는 `Clustered Index`와 `Non-Clustered Index` 두 종류가 존재**한다. 



# Clustered Index

`Clustered Index`를 책으로 비유하자면 페이지를 알고 있어서 바로 해당 페이지를 펼치는 것과 같다.

`Clustered Index`는 **테이블의 데이터를 지정된 컬럼에 대해 물리적으로 데이터를 재배열**한다.
_Index를 생성할 때는 데이터 페이지 전체를 다시 정렬_


**데이터가 테이블에 삽입되는 순서에 상관없이** **`Index`로 생성되어 있는 컬럼을 기준으로 정렬**되어 **삽입된다.**
_데이터 삽입, 수정, 삭제시 테이블의 데이터를 정렬_


**Index Page를 `키값`과 `데이터 페이지 번호`로 구성**하고, 검색하고자하는 **데이터의 `키 값`으로** **`페이지 번호`를 검색하여 데이터를 찾는다. **


![](https://images.velog.io/images/gillog/post/bf8980a9-00b4-45c7-a643-7a16932e8013/28513.jpg)






**`Clustered Index`**는** 테이블 당 한개씩만 존재 가능**하다.
_그래서 테이블에서 Index를 걸면 가장 효율적일거 같은 컬럼을 `Clustered Index`로 지정_


테이블에 **데이터가 많이 저장된 상태**에서 **ALTER를 통해 `Clustered Index`를 추가**한다면, **많은 데이터를 정렬해야 해서 많은 리소스를 차지**하게 된다.
_사용자가 많은 시간에는 함부로 Clustered Index를 추가하면 안됨_





## Clustered Index 방식

![](https://images.velog.io/images/gillog/post/5e179f0b-2100-4b82-9260-f2870c5d811a/2716FA44512C6B9827.png)

`Clustered Index`를 구성하기 위해서 행 데이터를 해당 열로 정렬한 후에, `루트 페이지`를 만들게 된다.

**`Clustered Index`는 `루트 페이지`와 `리프 페이지`로 구성**되며, **`리프 페이지`는 데이터 그 자체**이다.
_즉, Index 자체에 데이터가 포함_


**`Clustered Index` 물리적으로 정렬**되어 있어 **검색 속도가 `Non-Clustered Index` 보다 더 빠르다.**
_**데이터의 입력/수정/삭제 시에도 정렬을 수행**하여 **입력/수정/삭제 속도는 더 느리다.**_


---
# Non-Clustered Index

`Non-Clustered Index`를 책으로 비유하자면 목차에서 찾고자 하는 내용의 페이지를 찾고나서 해당 페이지로 이동하는것과 같다.

![](https://images.velog.io/images/gillog/post/62d7cd1e-ca5b-42e5-bdfb-ef80f9aed741/28467.jpg)

**`Non-Clustered Index`**는 **물리적으로 데이터를 배열하지 않은 상태**로 **`데이터 페이지`가 구성**된다.
_테이블의 데이터는 그대로두고 지정된 컬럼에 대해 정렬시킨 인덱스를 만들 뿐이다._



`Non-Clustered Index`는 **`Clustered Index`보다 검색 속도는 느리지만, 데이터의 입력/수정/삭제는 더 빠르다.**

`Non-Clustered Index`는** 테이블 당 여러개 존재 가능**하다.
_함부로 남용하면 오히려 시스템 성능을 떨어뜨린다._




## Non-Clustered Index 방식

![](https://images.velog.io/images/gillog/post/f8b57040-6173-40dd-9860-8b09c4ac3723/1123E744512C6B9914.png)

`Non-Clustered Index`는 **`데이터 페이지`를 건드리지 않고, 별도의 장소에 `인덱스 페이지`를 생성**한다.

`Non-Clustered Index`의 `인덱스 페이지`는 `키값(정렬하여 인덱스 페이지 구성)`과 `RID`로 구성된다.

<br>

`인덱스 페이지`의 **`리프 페이지`에 Index로 구성한 열을 정렬**한 후 **위치 포인터(RID)를 생성**한다.

`Non-Clustered Index`에서 인덱스 자체의 **`리프 페이지`는 데이터가 아니라**, **데이터가 위치하는 포인터(RID)다. **
_`RID`는 '파일그룹번호-데이터페이지번호-데이터페이지오프셋'으로 구성되는 포인팅 정보._

위처럼 **`리프 페이지`(중간 레벨 인덱스 페이지)**들을 **생성**하고, 이 **`리프 페이지`를 찾기위한 `루트 페이지`(루트 레벨 인덱스 페이지)를 생성**한다.

검색하고자하는 **데이터의 키 값을 `루트 페이지`에서 비교**하여 **`리프 페이지` 번호를 찾고**, **`리프 페이지`에서 RID 정보로 실제 데이터의 위치로 이동**한다.
_ EX : 12 검색위해 `루트 페이지`에서 파일 그룹번호 1에 해당하는 `리프 페이지` 100번으로 이동,
`리프 페이지` 100번에서 12는 `데이터 페이지` 1000번의 `데이터페이지오프셋` 3으로 되어 있음,
데이터 페이지 1000번으로 이동 후 3번째 컬럼으로 이동.
12, asdf5 검색 완료_







---

# Clustered Index & Non-Clustered Index 통합 그림



![](https://images.velog.io/images/gillog/post/ae37c5e4-1050-45a1-8c60-8b228aa7cbc7/993319475C17BD2D01.jpg)

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[Clustered Index & Non-Clustered Index 차이[사람 냄새나는 개발 일기]](https://serverwizard.tistory.com/93)

[인덱스란? Clustered Index/ Non-Clustered Index란?[개발자]](https://s1107.tistory.com/38)

[[SQL] 인덱스 (클러스터, 비클러스터) 개념[mongyang]](https://mongyang.tistory.com/75)

[클러스터형 및 비클러스터형 인덱스 소개[MicroSoft SQL Docs]](https://docs.microsoft.com/ko-kr/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described?view=sql-server-ver15)

[NonClustered Index Structure[SQL ServerCentral]](http://www.sqlservercentral.com/articles/Indexing/136537)

[]()
