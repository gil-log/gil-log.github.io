---
title: "[MyBatis] iBatis, MyBatis 비교 정리(Dynamic Query)"
last_modified_at: 2020-11-19T23:56
categories: 
  - java
tags: 
  - 'MyBatis' 
  - 'iBatis' 
  - '동적 쿼리'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# iBatis와 MyBatis

`iBatis`( ~ 2.3)의 버전이 변경되면서 `MyBatis`(2.5 ~)로 변경이 되었다.
_Apache project팀에서 google code 팀으로 이동하면서 명칭이 변경_

변경된 사항들을 정리해보면 아래와 같다.

## Java 요구 버전

Java 요구버전도 `iBATIS`는 JDK 1.4 이상에서 사용 가능하지만, `MyBatis`는 JDK 1.5 이상에서 사용 가능하도록 변경되었다.
_MyBatis 3.2 이상 버전은 JDK 1.6 이상 요구_

## 패키지 내부 구조

패키지 내부구조도 변경되었는데 `iBATIS`의 패키지 구조 `com.ibatis.*`에서 `MyBatis` 패키지 구조`org.apache.ibatis.*`로 변경 되었다.

## sqlMap.xml 내부 구조

`sqlMap.xml`도 `Mapper.xml`로 변경되며 내부구조도 바뀌었는데 기존의 `parameterMap`에서 `parameterType`으로 변경되었고, dtd역시 `http://mybatis.org/dtd/mybatis-3-mapper.dtd`로 변경 되었다.

사용 용어 역시 변경 되었는데, 기존의 `iBatis`와 `MyBatis`를 비교해보면 아래와 같다.

|iBatis|MyBatis|
|:--:|:--:|
|SqlMapConfig|Configration|
|sqlMap|Mapper|
|parameterMap|parameterType|
|resultClass|resultType|
|#var#|#{var}|
|`$var$`|`${var}`|

## Maven pom.xml 추가 방식

Maven 의존성 추가도 아래와 같이 변경 되었다.

```
<!-- iBATIS pom.xml -->
<dependency>
    <groupid>org.apache.ibatis</groupid>
   <artifactid>ibatis-sqlmap</artifactid>
   <version>2.3.4.726</version>
</dependency>

<!-- -->

<!-- MyBatis pom.xml -->
<dependency>
    <groupid>org.mybatis</groupid>
   <artifactid>mybatis</artifactid>
   <version>3.4.5</version>
</dependency>
 
<dependency>
   <groupid>org.mybatis</groupid>
   <artifactid>mybatis-spring</artifactid>
    <version>1.3.1</version>
</dependency>
```
## Annotation 도입
기존 `iBatis`에서 필요하던 `sqlMapClient` DI 설정이 필요 없어지고, `Bean id sqlSessionFactory`, `sqlSessionTemplate`만 지정해 주면 되도록 변경되었다.

## NameSpace
기존 `iBatis`에서는 `<sqlMap namespace = "ibatisDAO">`와 같이 사용하였지만, 
`MyBatis`에서는 `<mapper namespace = "com.gil.log.MybatisMapper">`과 같이 변경되었다.


---

# Dynamic Query

`iBatis`와 `MyBatis`에서 지원하는 **`Dynamic Query`는 상황에 따라 분기 처리를 통해 SQL을 동적으로 만드는 것**이다.

`iBatis`와 `MyBatis`에서 `Dynamic Query`를 사용하기 위해 쓰는 tag들은 아래와 같다.


## iBatis Dynamic Query Tag

iBatis  Dynamic Query Tag에서 사용되는 속성들은 아래와 같다.

`prepend` : 태그 조건에 일치하여 sql문에 선행하여 붙을 속성

`property` : 매개 변수 명

`compareProperty` : 비교할 다른 매개 변수 명

`compareValue` : 비교 대상이 될 값


### `<isEqual>`

property의 값이 같을때만 태그내 쿼리를 실행한다.
```
<!-- useYn 이 Y 일때만 EQUIP_TYPE =1 조건을 실행 -->
WHERE 1=1
 <isEqual prepend="AND" property="useYn" compareValue="Y">
        EQUIP_TYPE = 1
</isEqual>
```

### `<isNotEqual>`

property의 값이 같지 않을 때만 태그내 쿼리를 실행한다.

```
<!-- useYn 이 N이 아닐 때만 EQUIP_TYPE=1 조건을 실행 -->
WHERE 1=1
<isNotEqul prepend="AND" property="useYn" compareValue="N">
            EQUIP_TYPE = 1
</isNotEqual>
```

### `<isGreaterThan>`

property의 값이 비교값 보다 클 경우 쿼리를 실행한다.
```
<!-- age의 값이 19보다 클 경우 JOIN_YN = 'Y' 조건을 실행 -->
WHERE 1=1
<isGreaterThan prepend="AND" property="age" compareValue="19">
             JOIN_YN = 'Y'
</isGreaterThan>
```

### `<isGreaterEqual>`

property의 값이 비교값보다 크거나 같을 경우 쿼리를 실행한다.

```
<!-- age 값이 18 이거나 이보다 클경우 JOIN_YN='Y' 조건을 실행 -->
WHERE 1=1
<isGreaterEqual prepend="AND" property="age" compareValue="18">
              JOIN_YN = 'Y'
</isGreaterEqual>

```


### `<isLessEqaul>`

property의 값이 비교값보다 작거나 같을 경우 쿼리를 실행한다.
```
<!-- age값이 18이거나 이보다 작을 경우 JOIN_YN = 'Y' 조건을 실행 -->
WHERE 1=1
<isLessEqual prepend="AND" property="age" compareValue="18">
            JOIN_YN = 'N'
</isLessEqual>
```

그 외의 단일 조건 태그들은 아래와 같다.

|Tag|Note|
|:--:|:--:|
|`<isPropertyAvailable>`|property값이 유효할 경우 쿼리를 실행|
|` <isNotPropertyAvailable>`|property값이 유효하지 않을 경우 쿼리를 실행|
|`<isNull>`|property값이 null일 경우 쿼리를 실행|
|`<isNotNull>`|property값이 null이 아닐 경우 쿼리를 실행|
|`<isEmpty>`|property값이 비어있을경우 쿼리를 실행|
|`<isNotEmpty>`|property값이 비어있지 않을 경우 쿼리를 실행|


### `<isParameterPresent>`

parameter가 있을 경우 쿼리를 실행한다.

```
<!-- parameter값이 넘어왔을 경우에만 WHERE 조건이 붙는다. -->
<isParameterPresent prepend="WHERE"> 
          1=1
</isParameterPresent>
```

### `<isNotParameterPresent>`

parameter가 없을 경우 쿼리를 실행한다.

```
<!-- parameter값이 없을 경우에만 TYPE = 'DEFAULT' 쿼리 실행 -->
WHERE 1=1
<isNotParameterPresent prepend="AND">
         TYPE = 'DEFAULT'
<isNotParameterPresent>
```

### `<iterate>`

Parameter로 배열을 받아 쿼리를 실행한다.

```
<!-- 배열의 값을 빼내어 콤마로 구분하여 괄호 '(' , ')'안에 넣는다.
ex) ('111', '222', 333', '444')  -->
WHERE 1=1
<isNotEmpty prepend="AND" property="empIdArray">
       EMP_ID IN 
       <iterate open="(" close=")" conjunction="," property="empIdArray">#empIdArray[]#</iterate>
</isNotEmpty>
```


### `<dynamic>`

하위 tag에 일치되는 내용이 존재 할 경우 쿼리를 실행한다.
```
<!-- empId 파라메터 값 123이라면 <isEqual>태그의 prepend는 생략되고
WHERE 절이 붙어 WHERE VACATION = 'TRUE' 쿼리가 실행 -->
<dynamic prepend="WHERE">
        <isEqual prepend="AND" property="empId" comapareValue="123">
                    VACATION = 'TRUE'
        </isEqual>
</dynamic>
```

---

## MyBatis Dynamic Query Tag

### `<if>`

`<if>` tag는 `iBatis`에서 사용하던 `isEqual`, `isNotEqual`, `isNull`, `isNotNull`, `isEmpty`, `isNotEmpty`를 하나로 표현한다.


<if>는 test 속성을 만족하면 해당 쿼리를 추가한다.
 
test 속성은 true/false를 판단할 수 있는 boolean 타입의 조건식이 필요하다. 

조건식을 작성할 때 parameter 기준으로 하는 경우가 많은데 parameter를 참조할 때는 SQL에서 처럼 #{파라미터명}의 형태가 아닌 그냥 파라미터명만 사용함을 주의해야 한다.


```
WHERE 1=1
<if test="empId != null">
       AND EMP_ID = #{empId}
</if>
```

`<if>` tag 사용 시에는 따옴표를 주의하여 문자열을 비교해야 한다.
```
<!-- 정상 작동하지 않음 -->
<if test="name.equals('kim')"></if>
<!-- 정상 작동 -->
<if test='name.equals("kim")'></if> 
```

아래는 `<if>` tag를 활용한 다양한 예시들이다.
  
<br>

#### 문자열 비교  
  
  
paraName1 이라는 파라미터가 null이 아니면서 값이 "test"와 동일한가?
```
 <if test='paraName1 != null  and(paraName1 eq "test".toString())'> </if>
```

paraName1 이라는 파라미터가 "all" 이라는 문자와 동일 하지 않은가?
```
<if test='!paraName1.equals("all")'>  </if>
```
  
#### 대소문자 관계없이 비교

paraName1 이라는 파라미터가 null이 아니면서 값이 "test" or "TEST"와 동일한가?
```
<if test='paraName1 !=null  and paraName1.equalsIgnoreCase("test")'> </if>
```
  
#### 특정 값 비교
  
  
paraName1 이라는 파라미터의 값이 "Y"인지 검사할 경우
```
<if test='paraName1== "Y"'></if>
``` 

paraName1 이라는 파라미터의 값이 공백인지 검사할 경우
```
<if test='paraName1 == " "'></if>
```
주의 : 작은 따옴표가 밖에 있어야 한다.

 
#### 숫자 비교
  
paraName1 이라는 파라미터의 값이 3보다 큰가?
```
<if test='paraName1 > 3'></if>  
```
paraName1 이라는 파라미터의 값이 3보다 크거나 같은가?                                     
```
<if test='paraName1 >= 3'></if>
```
paraName1 이라는 파라미터의 값이 3보다 작은가?  
```
<if test='paraName1 < 3'></if>
```
paraName1 이라는 파라미터의 값이 3보다 작거나 같은가?
```
<if test='paraName1 <= 3'></if>
```
paraName1 이라는 파라미터의 값이 숫자로 된 문자열일 경우
```
<!-- 비교할 값을 쌍 따옴표로 묶어준다. -->
<if test='paraName1 > "3"'></if>

```

 

#### 주의 사항

요소 유형 "null"과(와) 연관된 "test" 속성의 값에는 '<' 문자가 포함되지 않아야 한다.

이러한 예외가 발생하였다면 원인은 

if 태그 안에 ">" 괄호를 인식하지 못하는 거다. CDATA를 써도 적용되지 않는다.

일단 원인은 ">" 괄호를 XML Parsing으로 인식한 건데, XML Parsing을 Text로 바꿔주는 CDATA 마저 적용되지 않는 현상을 해결하려면 아래와 같은 대체식을 사용해야 한다.
  
  
|기호|대체식|예제|
|:--:|:--:|:--:|
|<|lt|`<if test="paraName1 lt 0">`|
|>|gt|`<if test="paraName1 gt 0">`|
|<=, =<|lte|`<if test="paraName1 lte 0">`|
|>=, =>|gte|`<if test="paraName1 gte 0">`|
  
  
#### or문
  
||가 아닌 or로 써야 한다.

or (O)

|| (X)

paraName1 값이 Y이거나 paraName2의 값이 N인 경우

 
```
<if> 문

<if test='paraName1 == "Y" or paraName2 == "N"'></if>
<choose> 문

<choose>
<when test='paraName1 == "Y" or paraName2 == "N"'>
</choose>
```

`||` 를 사용하려면 `amp;`를 붙여 주어야한다.

`|amp;|amp;`
```
<if test='paraName1 == "Y" |amp;|amp; paraName2 == "N"'></if>
```  
  
#### and문
  
&&가 아닌 and로 써야한다.

and (O)

&& (X)

 

paraName1 값이 Y이고 paraName2의 값이 N인 경우
```
<if> 문

<if test='paraName1 == "Y" and paraName2 == "N"'></if>
<choose> 문

<choose>
<when test='paraName1 == "Y" and paraName2 == "N"'>
</choose>
``` 

`&&` 를 사용려면 `amp;`를 붙여 주어야 한다.
`&amp;&amp;`
```
<if test='paraName1 == "Y" &amp;&amp; paraName2 == "N"'></if>
```
  
#### null,nullString 체크
```
<if test='paraName1 == null'></if>
<if test="paraName1 == null"></if>

<if test="!paraName1.equals('') and paraName1!=null"> </if>

<if test="paraName1!=null and !paraName1.equals('')"> </if>  
```

### `<choose>, <when>, <otherwise>`

case 문과 같이 case에 따라 조건이 달라질 때 사용한다.
  
**`<if>` 구문은 하나의 조건에 대한 판단만 가능하고 else 또는 else if에 대한 구문은 존재하지 않는다.** 
  
여러 배타적인 조건에 대해서 처리하기 위해서는 `<choose> - <when/> -<when/> - <otherwise/> </choose>` 구문을 사용한다.
  
if ~ else if ~ else if ~ else와 같은 내용이라고 보면 된다.


```
<!--
prameter
searchCondition이 title 이면 AND TITLE LIKE #{title} 을
searchCondition이 content이면 AND CONTENT LIKE #{content} 를
아니면 AND DEL_YN = 'N' 을 조회
-->
WHERE 1=1
<choose>
   <when test = "searchCondition == 'title'">
       AND TITLE LIKE #{title}
   </when>

   <when test = "searchCondition == 'content'">
       AND CONTENT LIKE #{content}
   </when>

   <otherwise>
       AND DEL_YN = 'N'
   </otherwise>
</choose>
```

### `<where>, <trim>`

iBatis의 `<dynamic>`과 같이 조건에 따라 where 절을 추가 할 때 사용한다.

`<where>`는 엘리먼트 이름에서 풍기듯 where 조건절을 만드는데 특화된 엘리먼트이다.
  
`<if>`나 `<choose>`계열에서는 where 절이 각 조건마다 반복해서 등장하는데 `<where>`를 사용하면 하위 엘리먼트에서 생성한 내용일 있을 경우에만 where를 붙여주고 없으면 무시한다.
  
`<where>`내부에는 조건을 표현할 수 있는 `<if>`나 `<choose>`가 사용될 수 있다.
  
 <br>
  
trim은 속성이 많아서 복잡해보이지만 where의 단점을 극복할 수 있는 tag이다.
  

trim에서 사용하는 속성들은 아래와 같다.
  
  

`prefixOverrides`: 하위 엘리먼트 처리 후 내용의 맨 앞에 해당 문자열이 있다면 지워버림

`suffixOverrides`: 하위 엘리먼트 처리 후 내용의 맨 뒤에 해당 문자열이 있다면 지워버림

`prefix`: 하위 엘리먼트 처리 후 내용이 있다면 가장 앞에 붙일 내용

`suffix`: 하위 엘리먼트 처리 후 내용이 있다면 가장 뒤에 붙일 내용
  
  
```
<!-- trim태그를 위와 같이 사용하면 맨 첫번째 AND나 OR을 WHERE로 바꾼다. -->
  SELECT COUNT(*)
    FROM MST_USER
    <trim prefix="WHERE" prefixOverrides="AND|OR">
         <if test="id != null">
               AND USER_ID = #{id}
         </if>
         <if test="pw != null">
               AND USER_PW = #{pw}
         </if>
     </trim>
```

### `<set>`

동적으로 updtae 구문을 만들때 사용한다.
`<set>`은 update 문장에서 null 여부에 따라서 동적으로 할당 할 수 있다.
  
```
<!--
동적으로 set 키워드를 붙히고 불필요한 콤마를 제거한다.
<trim prefix="SET" suffixOverrides=","> </trim>과 같다. 
-->
UPDATE MST_USER
   <set>
       <if test="email != null">EMAIL = #{email},</if> 
       <if test="address != null">ADDRESS = #{address},</if>
       <if test="phone != null">PHONE = #{phone},</if
    </set> 
WHERE USER_ID = #{id}
```

### `<foreach>`
배열 타입의 parameter를 받을 때 사용한다.

배열이나 리스트의 경우 parameterType="Map" 으로 설정하고,
List일 경우 collection="list", 배열일 경우 collection="array" 로 설정한다.
_List나 Array의 경우 Mybatis 내부단에서 Map으로 치환_

`<foreach>`에서 사용되는 속성들은 아래와 같다.
         
`collection` : 값 목록을 가진 객체로 배열 또는 List

`item` : collection 내의 개별 값을 나타내는 변수 이름

`open` : 해당 블럭을 시작할 때 사용할 기호로 주로 '('

`close` : 해당 블럭을 종료할 때 사용할 기호로 주로 ')'

`separator` : 각 item을 구분할 분리자 기호로 주로 ','
         
```
<!--
배열의 값을 빼내어 콤마로 구분하여 괄호 '(' , ')' 내에 넣는다.
ex) ('111', '222', 333', '444') 
collection : 파라메터로 받은 배열변수의 명칭
item : collection의 alias
-->
<trim prefix="WHERE" prefixOverrides="AND|OR">
<if test="empIdArray != null">
      AND EMP_ID IN 
      <foreach item="empIdArray" index="index" collection="list" open="(" separator="," close=")">#{item}
      </foreach>
</if>
```

### `<bind>`

쿼리에서 사용할 변수를 생성한다.
```
<!-- title parameter를 받아 searchKeyword라는 변수에 저장하고 이를 쿼리에서 활용한다. -->
<bind name="searchKeyword" value="'%'+title+'%'"/>

SELECT *
  FROM BOARD
 WHERE TITLE LIKE #{searchKeyword}
```

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[iBATIS/MyBatis]iBATIS와 MyBatis의 차이[seon_u]](https://sdevstudy.tistory.com/18)

[iBatis, myBatis 동적 태그 비교 정리 Dynamic SQL[알짜배기 프로그래머]](https://aljjabaegi.tistory.com/310)

[04. MYBATIS - 동적 쿼리[은서파의 랜선 강의장]](https://goodteacher.tistory.com/249)

[[MyBatis] 동적 쿼리 if문 문법 총 정리[.java의개발일기]](https://java119.tistory.com/42)

[]()

[]()
