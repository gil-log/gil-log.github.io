---
title: "[MySQL] 이미 존재하는 Column에 참조 제약조건 추가하기(Constraint, Reference, Foreign Key)"
last_modified_at: 2021-08-08T23:44
categories: 
  - database
tags: 
  - 'Constraint' 
  - 'mysql' 
  - 'reference'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

**특정 테이블에 이미 존재하는 Column**에 **다른 테이블 Column과 참조 관계를 맺기 위해선 아래 Query**를 날려주면 된다.

```mysql
ALTER TABLE 자식테이블명 ADD CONSTRAINT 제약조건명 
FOREIGN KEY(자식컬럼) REFERENCES 부모테이블명(부모컬럼)
```


**실제 테이블 적용 Query 예제는 아래**와 같다.

```mysql
ALTER TABLE TB_DEBATE ADD CONSTRAINT debate_user_fk 
FOREIGN KEY(U_ID) REFERENCES TB_USER(U_ID)
```

![](https://images.velog.io/images/gillog/post/2c8f6107-e2ba-48f6-ba71-1939960f73bb/image.png)