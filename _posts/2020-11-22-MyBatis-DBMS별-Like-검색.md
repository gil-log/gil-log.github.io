---
title: "[MyBatis] DBMS별 Like 검색"
last_modified_at: 2020-11-22T13:55
categories: 
  - java
tags: 
  - 'MyBatis'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# Like 검색

MyBatis에서 Parameter를 통해 검색을 하고 싶다면, DBMS에 맞게 문자열 합치기 함수를 사용하면 된다.

아래는 대표적인 MySQL, Oracle, MS-SQL에서 MyBatis Like 검색 방법이다.


## MySQL

```sql
search_column like CONCAT('%',#{keyword},'%')
```

## Oracle
```sql
search_column like '%' ||  #{keyword} || '%'
```

## MSSQL
```sql
search_column like '%' + #{keyword} + '%'
```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[MyBatis Like 검색시 처리방법[과일가게 개발자]](https://fruitdev.tistory.com/60)

[]()

[]()

[]()

[]()

[]()
