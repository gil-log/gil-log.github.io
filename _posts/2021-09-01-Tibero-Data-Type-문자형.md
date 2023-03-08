---
title: "[Tibero] Data Type - 문자형"
last_modified_at: 2021-09-01T06:57
categories: 
  - database
tags: 
  - 'Tibero' 
  - 'data type' 
  - '문자형'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import
[Tibero SQL 참조 안내서](https://www.tmaxdata.com/img/service/pdf/Tibero%205%20SQL%20Reference%20Guide%202.1.4.pdf)

---


**`Tibero`에서는 SQL 표준에 기반한 Data Type을 제공**한다.

이 중 **문자열을 표현하는 Data Type인 문자형 Type에는 아래 Type들**이 있다.


- CHAR
- VARCHAR
- VARCHAR2
- NCHAR
- NVARCHAR
- NVARCHAR2
- RAW
- LONG
- LONG RAW

---

# CHAR


**`CHAR` Type은 문자열을 저장하는 Data Type으로, 고정된 문자열 길이**를 가진다.

```
CHAR(size [BYTE|CHAR])
```


### 문자열은 최대 2,000Byte나 2,000자 까지 선언


**`CHAR` Type은 최대 2,000Byte나 2,000자 까지 선언**할 수 있고,

**변환된 문자열 길이**가 **2,000Byte나 2,000자가 넘어가면 Error가 발생**한다.


**`Tibero`에서는 `CHAR` Type으로 정의한 Column에 입력된 문자열**을,

**DB 문자 집합에 맞게 변환하여 저장**하는데 **이때 2,000 Byte나 2,000자를 초과해선 안된다.**

### 문자열 길이는 Byte와 문자를 기준으로 지정

**`CHAR` Type 선언 시 `CHAR(10 BYTE)`, `CHAR(10 CHAR)` 형태로 선언**한다.
_BYTE로 선언할 경우엔 10바이트까지 저장, 
CHAR로 선언할 경우엔 10자까지 저장_


만약 **`CHAR(10)` 형태로 선언하면 기본 값은 BYTE로 문자열 길이가 지정**된다.


**`CHAR(10 CHAR)` 형태로 선언 할 때**는, **실제 Column 길이**는 **DB에서 사용하는 문자 집합에 따라** 달라진다.
_**한 문자가 몇 BYTE로 표현되는지**에 따라_


### SQL에서 CHAR Type 값에는 작은 따옴표(')를 사용

**`CHAR_COLUMN = 'charVal'` 형태로 사용**한다.

### 문자열 길이가 0인 값은 NULL로 인식


### Example

```
GILLOG_NAME CHAR(10)
```

위와 같은 Column은 **10 Byte의 문자열 길이**를 가지게 되고,

만약 `GILLOG_NAME = 'gillog'` 와 같은 문자열을 입력할 경우,

실제로 **저장되는 문자열은 `gillog____`로 4개의 공백 문자**가 채워지게 된다.
_**선언된 길이보다 짧은 문자열이 입력**될 경우, **남은 부분은 공백 문자(Space)**로 채워짐_


---

# VARCHAR

**`VARCHAR` Type도 `CHAR` Type처럼 문자열을 저장하는 Data Type**이다.

**`CHAR` Type과 다른점**은 **문자열 길이가 일정하지 않은 가변 길이를 가진다는 점**이다.

```
VARCHAR(size [BYTE|CHAR])
```

### 문자열은 최대 4,000 Byte나 4,000자 까지 선언

**변환된 문자열 길이**가 **4,000 Byte나 4,000자를 초과할 경우 Error가 발생**한다.

**`VARCHAR` Type의 Column에 입력된 문자열**을 **DB 문자 집합에 맞게 변환하여 저장**하는데,

**변환된 문자열**은 **4,000 Byte나 4,000자를 초과해서는 안 된다.**



### 문자열 길이는 Byte와 문자를 기준으로 지정


**`VARCHAR` Type 선언 시 `VARCHAR(10 BYTE)`, `VARCHAR(10 CHAR)` 형태로 선언**한다.
_BYTE로 선언할 경우엔 10바이트까지 저장, 
CHAR로 선언할 경우엔 10자까지 저장_


만약 **`VARCHAR(10)` 형태로 선언하면 기본 값은 BYTE로 문자열 길이가 지정**된다.


**`VARCHAR(10 CHAR)` 형태로 선언 할 때**는, **실제 Column 길이**는 **DB에서 사용하는 문자 집합에 따라** 달라진다.
_**한 문자가 몇 BYTE로 표현되는지**에 따라_



### SQL 문장에서 VARCHAR Type 값에는 작음 따옴표(')사용


**`VARCHAR_COLUMN = 'varCharVal'` 형태로 사용**한다.


### 문자열 길이가 0인 값은 NULL로 인식


### Example

```
GILLOG_NAME VARCHAR(10)
```

위와 같은 Column은 **10 Byte의 문자열 길이**를 가지게 되고,

만약 `GILLOG_NAME = 'gillog'` 와 같은 문자열을 입력할 경우,

실제로 **저장되는 문자열은 `gillog`로 6글자가 저장**된다.

**`VARCHAR` Type**은 **선언된 문자열 길이 범위 내**에서,

**입력 문자열 길이와 저장 문자열 길이가 같다.**
_공백 문자(Space)가 들어가지 않음._


---

# VARCHAR2

**`VARCHAR2` Type은 `VARCHAR` Type과 동일**하다.

---

# NCHAR


**`NCHAR` Type**은 **유니코드 문자열 저장을 위한 Type**으로, **항상 고정된 문자열 길이**를 갖는다.

```
NCHAR(size)
```

### CHAR Type과 유사, 문자열 길이가 문자 기준

**`NCHAR` Type의 길이**는 **DB 다국어 문자 집합에 따라** 다른데,

**`UTF8`의 경우 size의 최대 3배**,

**`UTF16`의 경우 size의 최대 2배가 저장**된다.

**기존 `CHAR(6)`에는 한글 기준**, **길이가 3인 문자열까지만 저장**되지만,

**`NCHAR(6)`에는 한글 기준**에서도 **6글자 까지 저장이 가능**하다.

### NCHAR Type 문자열의 최대 길이는 2,000자

### SQL 문장에서 NCHAR Type의 값에는 항상 작은 따옴표(')를 사용

### 문자열 길이가 0인 값은 NULL로 인식

---

# NVARCHAR

**`NVARCHAR` Type**은 **`NCHAR`와 같이 유니코드 문자열 저장을 위한 Type**이다.

**`VARCHAR`와 마찬가지**로 **문자열 길이가 일정하지 않은 가변 길이**를 가진다.


### VARCHAR Type과 유사, 문자열 길이가 문자 기준

**`NCHAR`와 마찬가지**로 **`NVARCHAR` Type의 길이**는 **DB 다국어 문자 집합에 따라** 다른데,

**`UTF8`의 경우 size의 최대 3배**,

**`UTF16`의 경우 size의 최대 2배가 저장**된다.

**기존 `VARCHAR(6)`에는 한글 기준**, **길이가 3인 문자열까지만 저장**되지만,

**`NVARCHAR(6)`에는 한글 기준**에서도 **6글자 까지 저장이 가능**하다.


### NVARCHAR Type 문자열 최대 길이는 4,000자


### SQL 문장에서 NVARCHAR Type 값 표현에는 항상 작은 따옴표(') 사용

### 문자열 길이가 0인 값은 NULL로 인식

---

# NVARCHAR2

**`NVARCHAR2` Type**은 **`NVARCHAR` Type과 완전히 동일**하다.


---

# RAW

**`RAW` Type**은 **임의 Binary Data 저장 Data Type**이다.

이때 **Binary Data**는 **선언된 최대 길이내 임의 길이**를 가진다.

**`RAW` Type**이 **`CHAR`, `VARCHAR` Type과 다른점**은,

**`RAW` Type**의 경우 **Data 중간에 NULL 문자('\0')**가 올 수 있다.
_**`CHAR`, `VARCHAR`**는 **Data 끝을 표현할때 NULL 문자('\0') 사용**_

<br>

**`RAW` Type**은 **NULL 문자('\0')로 Data 끝을 나타낼 수 없어**, 

항상 **길이 정보를 같이 저장** 한다.

```
RAW(size)
```

### 최대 2,000 Byte 까지 선언

### 선언된 길이 내 가변 길이

### Data 중간 NULL 문자('\0') 올 수 있음

### 입출력 수행 시, RAW Type Data는 16진수로 표현

예를 들어 **4 Byte Data**는 **16 진수로 '012345AB'로 표현** 되며,

**필요한 경우 맨 앞이 0으로 시작**되야 한다.


---

# LONG

**`LONG` Type**은 **`VARCHAR` Type을 확장한 Data Type**이다.

**`VARCHAR` Type과 마찬가지로 문자열 저장이 가능**하다.

``` 
LONG
```

### 최대 2GB까지 선언

### Table내 한 Column에만 선언 가능

### LONG Type Column 컬럼은 Index 설정 불가

### LONG Type Column을 포함한 Row가 Disk에 저장 될 때, 다른 Column 값과 동일한 Disk Block에 저장, 길이에 따라 여러 Disk Block에 걸쳐 저장 가능


### LONG Type Data에 접근할 때는 항상 순차적으로만 접근, 임의 위치 연산 불가

### LONG Type Column이 있는 Table에는 새로운 Column 추가 불가

---


# LONG RAW

**`LONG RAW` Type**은 **`RAW` Type을 확장한 Data Type**이다.

**`RAW` Type과 마찬가지로 Binary Data가 저장**된다.

```
LONG RAW
```

### 최대 2GB까지 선언

### Table내 한 Column에만 선언

### LONG RAW Type Column에는 Index 설정 불가

### LONG RAW Type Column을 포함한 Row가 Disk에 저장 될 때, 다른 Column 값과 동일한 Disk Block에 저장, 길이에 따라 여러 Disk Block에 걸쳐 저장 가능


### LONG RAW Type Data에 접근할 때는 항상 순차적으로만 접근, 임의 위치 연산 불가