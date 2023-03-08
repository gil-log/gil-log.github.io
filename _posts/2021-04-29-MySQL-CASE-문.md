---
title: "[MySQL] CASE 문"
last_modified_at: 2021-04-29T07:02
tags: 
  - 'case' 
  - 'migration' 
  - 'mysql' 
  - 'select'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[MySQL] CASE 기본 사용법[확장형 뇌 저장소]](https://extbrain.tistory.com/46)

[]()

[]()

[]()

[]()

[]()

<br>

# CASE 문

**MySQL에서 CASE문**은 **다른 프로그래밍 언어에서 Switch 문과 비슷**하다.
_**다수의 조건에 하나 반환 값은 불가**하다, **[EX]WHEN 조건1, 조건2 THEN 결과1 - 불가**_

MySQL에서 CASE문 문법은 아래와 같다.

```sql
CASE
	WHEN 조건1
    THEN '반환값'
    WHEN 조건2
    THEN '반환값'
    ELSE '조건1, 조건2에 해당 안되는 경우 반환 값'
END
```

이때 **`WHEN`과 `THEN`은 같이 묶음으로 사용**해야 한다.

**`WHEN`과 `THEN`의 개수는 상관없다.**
_다수 사용 가능_

**ELSE가 존재하면 모든 조건에 해당하지 않는 경우에 반환 값을 설정**할 수 있다.

**ELSE가 존재하지 않고, 조건에 맞지 않아서 반환 값이 없으면 NULL를 반환**한다.


실제 사용 예시는 아래와 같다.

```sql
SELECT 
	id
    , CASE
    	WHEN flag = '0'
        THEN '없음'
        WHEN flag = '1'
        THEN '있음'
        ELSE '모름'
      END as flag_case
      , name
FROM GILLOG
```
