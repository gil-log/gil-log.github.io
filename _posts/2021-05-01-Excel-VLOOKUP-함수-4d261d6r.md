---
title: "[Excel] VLOOKUP 함수"
last_modified_at: 2021-05-01T07:17
categories: 
  - excel
tags: 
  - 'VLOOKUP' 
  - 'excel'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[엑셀 Excel VLOOKUP함수, 원하는 값 찾을 사용[녹두장군]](https://mainia.tistory.com/5651)

<br>

# VLOOKUP

**`VLOOKUP`은 Excel에서 표나 범위에서 특정 행을 기준으로 항목을 찾을 때 사용하는 함수**이다.

**`VLOOKUP` 함수의 인자와 사용법은 아래**와 같다.

<br>

## VLOOKUP(조회하려는 값, 값 조회 범위, 반환 값 열 번호, 정확한 일치 or 유사 일치(TRUE, FALSE))

<br>

실제로 **`VLOOKUP`을 활용** 할 때는 아래와 같이 **`Wild Card`를 활용하여 문자가 포함된** 컬럼을 찾아 **그 컬럼의 다른 항목 값을 가져오는 등 다양하게 활용 가능**하다. 

![](https://images.velog.io/images/gillog/post/ecfed4e1-44ea-4cb7-a286-b85ddfe55835/20210501_160305.png)


```
G컬럼
=VLOOKUP("*"&$B$2&"*", $C$2:$E$5, 3, FALSE)
```

여기서 **`VLOOKUP`의 첫 번째 인자의 의미는 B2 셀의 값 앞 뒤에 `WILD CARD` `*` 을 `&`을 이용해 붙여주며 B2 셀이 포함된 컬럼을 조회하겠다는 의미**이다.

**두 번째 인자에서는 C2 ~ E5셸의 값을 조회 대상의 범위로 설정하겠다는 의미**이다.

이때 최종적으로 가지고 오고 싶은 컬럼을 포함 해주어야 한다.

**세 번째 인자는 최종 출력 값의 순서 Index로 만약 C2 셀에서 검색 결과가 일치**한다면, **그 시작 컬럼을 INDEX 1로** 잡고, 
**3번째 INDEX에 있는 값을 최종 출력으로 정하겠다는 의미**이다.
_**D2 셀에서 검색 결과가 일치해도 이때 D2 셀은 같은 행인 C2부터 검색 범위이므로 INDEX가 2 이고 동일하게 INDEX 3인 E2 셀을 출력**한다._

**마지막 네 번째 인자는 `TRUE`를 사용하면 정확히 일치하는 것을 찾겠다는 의미이고, `FALSE`를 사용하면 정확히 같지 않더라도 제일 비슷한 값이라도 사용하겠다는 의미**이다.