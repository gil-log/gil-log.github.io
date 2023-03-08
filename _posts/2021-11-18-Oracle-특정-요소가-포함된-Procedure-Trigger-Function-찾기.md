---
title: "[Oracle] 특정 요소가 포함된 Procedure, Trigger, Function 찾기"
last_modified_at: 2021-11-18T02:42
categories: 
  - database
tags: 
  - 'Orcale' 
  - 'TRIGGER' 
  - 'function' 
  - 'procedure'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
**수많은 Procedure, Trigger, Function 들로 구성된 DB**가 있을때,

**특정 Table 관련이나 특정 Column** 등과 같이

**특정 요소가 포함**된 **Procedure, Trigger, Function을 검색해야 할 때**가 있다.

<br>

**아래 쿼리 처럼 `USER_SOURCE` 테이블**에서 **`TYPE` Column에 검색 대상**이 될 

**항목들을 지정**하고, **`TEXT` Column에 검색할 특정 요소를 조회**하면,

**원하는 결과**를 얻을 수 있다.


```sql
SELECT *
FROM USER_SOURCE
WHERE TYPE IN('PROCEDURE', 'FUNCTION', 'TRIGGER')
AND TEXT LIKE '%검색하고싶은내용%'
```