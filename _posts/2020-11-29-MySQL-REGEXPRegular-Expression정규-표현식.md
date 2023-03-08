---
title: "[MySQL] REGEXP(Regular Expression(정규 표현식))"
last_modified_at: 2020-11-29T02:24
categories: 
  - database
tags: 
  - 'mysql' 
  - 'regexp' 
  - 'regular expression' 
  - '정규 표현식'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# REGEXP ??

**`REGEXP`**는 **`LIKE`를 이용한 검색과 달리 `Regular Expression(정규 표현식)`를 이용해 검색**한다.

**`REGEXP`**를 사용하면 SQL에서 **정규표현식을 활용하여 기본 연산자보다 복잡한 문자열 조건을 걸어 데이터를 검색**할 수 있다.

하지만 **정규표현식 검색을 이용할 때 절대 사용자에게 정규식 기능을 제공해선 안된다.**
_각종 오류를 포함할 수 있고 SQL Injection에 취약해지기 때문에,
정규표현식의 검색을 개발자가 미리 정한 테두리 안에서 행해져야 한다._

<br>

## Regular Expression(정규 표현식) 이란?

**정규 표현식은 특정한 규칙을 가진 문자열의 집합을 표현하는데 사용하는 형식 언어**이다.

**문자열을 처리하는 방법 중의 하나**로, **특정한 조건의 문자**를 **‘검색’**하거나 **‘치환’**하는 과정을 **매우 간편하게 처리할 수 있도록 해주는 수단**이다.

**정규 표현식은 SQL부터 스크립트 언어까지 다양한 곳에서 활용**할 수 있다.

**정규 표현식은 `Pattern`을 사용해서 문자열을 처리**한다.

**찾을 대상문자열에서** 정규 표현식을 사용해** 해당 `Pattern`과 일치하는 문자열을 검색하는 것**이다.

**`Pattern`과 일치하는 문자열을 찾은 이후 추출하거나 치환 할 수 있다.**

사용되는 Pattern의 종류는 아래와 같다.

### Matching

|Pattern|기능|예시|설명|
|:--:|:--:|:--:|:--:|
|.|문자 하나|"..."|문자열의 길이가 세 글자 이상인 것을 찾음.|
| I(수직선) | 또는 (OR). I(수직선)로 구분된 문자에 해당하는 문자열을 찾음.|"데이터I(수직선)데이타"|‘데이터’ 또는 ‘데이타’에 해당하는 문자열을 찾음.|
|[]	|[] 안에 나열된 패턴에 해당하는 문자열을 찾음.|	"[123]d"	|대상 문자열에서 ‘1d’ 또는 ‘2d’ 또는 ‘3d’인 문자열을 찾음.|
|^	|시작하는 문자열을 찾음.	|"^안녕"	|대상 문자열에서 ‘안녕’으로 시작하는 문자열을 찾음.|
|$	|끝나는 문자열을 찾음.	|"잘가$"	|대상 문자열에서 ‘잘가’로 끝나는 문자열을 찾음.|


### Numbers Limit

|Pattern|기능|예시|설명|
|:--:|:--:|:--:|:--:|
|*	|0회 이상 나타나는 문자	|"a*"	|‘a’가 0번 이상 등장하는 문자열을 찾음. ‘b’, ‘a’, ‘aa’ 모두 해당.|
|+	|1회 이상 나타나는 문자	|"국+"	|‘국’이 1번 이상 등장하는 문자열을 찾음. ‘한국’, ‘미역국’, ‘국거리’ 모두 해당.|
|{m,n}	|m회 이상 n회 이하 반복되는 문자	|"치{1,2}"	|‘치’가 1회 이상 2회 이하 반복하는 문자열을 찾음. ‘치커리’, ‘치카치카’ 모두 해당.|
|?	|0~1회 나타나는 문자	|"[가나다]?"	|‘가’ 또는 ‘나’ 또는 ‘다’가 0~1회 등장하는 문자열을 찾음. ‘가지마’, ‘나라’, ‘안녕’ 모두 해당.|


### String Group
|Pattern|기능|예시|설명|
|:--:|:--:|:--:|:--:|
|[A-z] 또는 [:alpha:] 또는 \a|	알파벳 대문자 또는 소문자인 문자열을 찾음	|"[A-z]+"	|대상 문자열에서 알파벳이 한 개 이상인 문자열을 찾음|
|[0-9] 또는 [:digit:] 또는 \d|	숫자인 문자열을 찾음	|"^[0-9]+"	|한 개 이상의 숫자로 시작하는 문자열을 찾음|


### Not
|Pattern|기능|예시|설명|
|:----:|:--:|:----:|:--:|
|[^문자]|	괄호 안의 문자를 포함하지 않은 문자열을 찾음|	"[^길로그]"	|‘길’ 또는 ‘로’ 또는 ‘그’를 포함하지 않는 문자열을 찾음. ‘길가’, ‘로그’, ‘그리고’ 모두 제외됨.|

---

# REGEXP 사용 예제

```sql
# '길' 또는 '로" 또는 '그'가 포함된 문자열을 찾고 싶을 때

# 정규표현식을 사용하지 않을 때
SELECT *
FROM tbl
WHERE data like '%길%'
OR data like '%로%'
OR data like '%그%'

# 정규표현식을 사용할 때
SELECT *
FROM tbl
WHERE data REGEXP '길|로|그'


# ‘안녕’ 또는 ‘하이’로 시작하는 문자열을 찾고 싶을 때

# 정규표현식을 사용하지 않을 때 
SELECT *
FROM tbl  
WHERE data LIKE '안녕%' OR data LIKE '하이%';

# 정규표현식을 사용할 때 
SELECT *
FROM tbl
WHERE data REGEXP ('^안녕|^하이');

-----------------------------------------------

# 길이 7글자인 문자열 중 2번째 자리부터 abc를 포함하는 문자열을 찾고 싶을 때

# 정규표현식을 사용하지 않을 때
SELECT *
FROM tbl
WHERE CHAR_LENGTH(data) = 7 AND SUBSTRING(data, 2, 3) = 'abc';

# 정규표현식을 사용할 때
SELECT *
FROM tbl
WHERE data REGEXP ('^.abc...$');

-----------------------------------------------

# 텍스트와 숫자가 섞여 있는 문자열에서 숫자로만 이루어진 문자열을 찾고 싶을 때

# 정규표현식을 사용하지 않을 때
SELECT *
FROM tbl
WHERE data LIKE ??????????

# 정규표현식을 사용할 때
SELECT *
FROM tbl
WHERE data REGEXP ('^[0-9]+$'); 
-- OR data REGEXP ('^\d$') 
-- OR data REGEXP ('^[:digit:]$');

```



<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[MySql,MariaDB] Like in 같이 쓰기 해결책 REGEXP[기타치는 개발자의 야매 가이드]](https://yamea-guide.tistory.com/entry/MySqlMariaDB-Like-in-%EA%B0%99%EC%9D%B4-%EC%93%B0%EA%B8%B0-%ED%95%B4%EA%B2%B0%EC%B1%85-REGEXP)

[정규표현식 (Regular Expression) 이해하기[by Yurim Koo]](https://yurimkoo.github.io/analytics/2019/10/26/regular_expression.html)

[[MySQL] 정규식을 이용한 검색 regexp[seobangnim]](https://steemit.com/mysql/@seobangnim/mysql-regexp)