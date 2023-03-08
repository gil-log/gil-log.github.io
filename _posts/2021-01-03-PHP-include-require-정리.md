---
title: "[PHP] include, require 정리"
last_modified_at: 2021-01-03T23:18
categories: 
  - php
tags: 
  - 'include' 
  - 'php' 
  - 'require'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[include 와 require 차이 :: include_once, require_once [PYH]](https://m.blog.naver.com/blogpyh/220114412283)

[php에서 include(), include_once(),require(), require_once() 사용법 및 차이점[이야기앱 세상]](https://appsnuri.tistory.com/439)

[]()

[]()

[]()

[]()

<br>


# include()

`include`는 특정한 파일을 현재 실행중인 스크립트에 포함시키고자 할 때 사용한다.



## include() 특징

  - 일반적인 document Embeded 방식이다. 

  - 이 문장을 만날 때 마다 매번 재평가되어 재실행된다.

  - include 문장을 만날때마다 지정한 파일을 포함한다.

  - loop나 if 문 등에서 사용하여 필요한 경우에만 파일을 포함하도록 할 수 있다.

  - 해당 구문에 도달해야만 읽어 온다.

  - Error발생시 Warning을 일으킨다.


## include_once()

**`include_once()`는 한번만 include 하는 경우에 사용**한다.


## include_once() 특징

`include_once()`는 `include()`와 대부분 동일한 수행을 한다.

하지만 문서에 **이미 로드된 동일 문서가 있다면, `include_once()`는 더이상 `include`하지 않는다.**

gil.php라는 파일에 function log() 라는 사용자 함수가 정의되어 있을때 gil.php를 **여기 저기서 include or require하게되면 중복된 함수 정의 에러가 발생**하게 된다. 

**이러한 문제를 막기위해 `include_once()`를 사용**한다.

---

# require()

require는 특정한 파일을 현재 실행중인 스크립트에 포함시키고자 할 때 사용한다.

`require`는 현재의 **스크립트에 포함시키려는 파일 또는 문서가 실제로 지정한 경로나 URL에 존재하지 않을경우**,

**`include`는 파일이나 문서가 존재하지않는다는 경고성 메시지를 출력하고 계속 파싱**하지만,

**`require`는 치명적인 에러가 발생했다고 메시지를 출력 후 해당 프로그램을 중지**시킨다. 


## require_once()

**`require_once()`는 한번만 require 하는 경우 사용**한다.

중복된 `require`를 방지하는 제어문으로 **중복된 함수 정의 에러를 막기 위해 사용**한다.


<br>

**`require_once()` or `include_once()`를 사용하면 이미 로딩된 파일은 중복해서 읽어 들이지 않는다.**


---

# include() vs require()

## 1. 조건적 삽입 가능 여부

**`include()`는 include가 호출될 때 지정한 파일을 삽입하여 특정 조건일 때 지정한 파일이 삽입되게 처리**할 수 있다.

_**조건을 만족하지 않는다면 파일 삽입 안됨**_

<br>

반면에 **`require()`는 무조건 파일을 삽입**하기 때문에 **특정 조건에 만족하지 않아도 지정한 파일을 삽입**한다. 

**`require()`는 조건에 따라 수행하는 것이 아니라 파일을 무조건 삽입해야 할 때 사용**한다.
_**조건을 만족하지 않아도 파일 삽입**_


<br>

## 2. 삽입 실패 시 프로그램 실행 여부
 
현재의 **스크립트에 포함시키려는 파일 또는 문서가 실제로 지정한 경로나 URL에 존재하지 않을경우**,

**`include()`는 파일이나 문서가 존재하지않는다는 경고성 메시지를 출력하고 계속 파싱**하지만,

**`require()`는 치명적인 에러가 발생했다고 메시지를 출력 후 해당 프로그램을 중지**시킨다. 