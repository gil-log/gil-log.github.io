---
title: "[Linux] grep command"
last_modified_at: 2021-01-12T22:46
categories: 
  - linux
tags:
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[리눅스 grep 명령어 사용법. (Linux grep command) - 리눅스 문자열 검색[개발자를 위한 레시피]](https://recipes4dev.tistory.com/157)

[]()

[]()


<br>



# grep


`grep`은 **입력으로 전달된 파일의 내용에서 특정 문자열을 찾고자할 때 사용하는 명령어**로 **리눅스에서 가장 많이 사용되는 명령어 중 하나**이다.


`grep` 명령어가 문자열을 찾는 기능을 수행한다고 해서, 단순히 문자열이 일치하는지 여부만을 검사하는 것은 아니다. 

`grep`은 파일의 문자열을 검색할 때, **문자열이 같은지(equal)만을 검사하는 수준을 넘어**,  단순 문자열 매칭이 아니라, **정규 표현식(Regular Expression)에 의한 패턴 매칭(Pattern Matching) 방식을 사용**한다.

## grep options
|옵션|설명|
|:--:|:--:|
|grep [OPTION...]| PATTERN [FILE...]|
|-E       | PATTERN을 확장 정규 표현식(Extended RegEx)으로 해석.|
|-F        | PATTERN을 정규 표현식(RegEx)이 아닌 일반 문자열로 해석.|
|-G        | PATTERN을 기본 정규 표현식(Basic RegEx)으로 해석.|
|-P        | PATTERN을 Perl 정규 표현식(Perl RegEx)으로 해석.|
|-e        | 매칭을 위한 PATTERN 전달.|
|-f        | 파일에 기록된 내용을 PATTERN으로 사용.|
|-i        | 대/소문자 무시.|
|-v        | 매칭되는 PATTERN이 존재하지 않는 라인 선택.|
|-w        | 단어(word) 단위로 매칭.|
|-x        | 라인(line) 단위로 매칭.|
|-z        | 라인을 newline(\n)이 아닌 NULL(\0)로 구분.|
|-m        | 최대 검색 결과 갯수 제한.|
|-b        | 패턴이 매치된 각 라인(-o 사용 시 문자열)의 바이트 옵셋 출력.|
|-n        | 검색 결과 출력 라인 앞에 라인 번호 출력.|
|-H        | 검색 결과 출력 라인 앞에 파일 이름 표시.|
|-h        | 검색 결과 출력 시, 파일 이름 무시.|
|-o        | 매치되는 문자열만 표시.|
|-q        | 검색 결과 출력하지 않음.|
|-a        | 바이너리 파일을 텍스트 파일처럼 처리.|
|-I        | 바이너리 파일은 검사하지 않음.|
|-d        | 디렉토리 처리 방식 지정. (read, recurse, skip)|
|-D        | 장치 파일 처리 방식 지정. (read, skip)|
|-r        | 하위 디렉토리 탐색.|
|-R        | 심볼릭 링크를 따라가며 모든 하위 디렉토리 탐색.|
|-L        | PATTERN이 존재하지 않는 파일 이름만 표시.|
|-l        | 패턴이 존재하는 파일 이름만 표시.|
|-c        | 파일 당 패턴이 일치하는 라인의 갯수 출력.|

# grep 사용 예제

`grep` 명령어를 사용하는 기본 방법은 아래와 같다.

```
grep [OPTION] [PATTERN] [FILE]
```


<br>


## 대상 파일 문자열 검색

```
grep "STR" [FILE.name]
```

**`grep` 명령에 문자열과 파일 이름을 지정**하여, **파일에서 문자열을 검색**할 수 있다. 

**문자열 검색 결과는 문자열이 포함된 라인 단위로 출력**된다.

<br>

아래는 "GILLOG.txt" 파일에서 "gil"이라는 문자열을 검색하고, 문자열이 존재하는 라인을 출력하는 예제이다.

_기본적으로 대소문자 구분_

```
$ cat GILLOG.txt
"GILLOG" is a developer blog.

The starting point was to record the facts that a man named gil learned by developing or studying his lazy habits.

GILLOG, which began with the very childish idea of taking a log of itself.

it is eventually the same place as development SNS for gil.

$ grep "PAT" FILE.txt
The starting point was to record the facts that a man named gil learned by developing or studying his lazy habits.

it is eventually the same place as development SNS for gil.
```

<br>


## 현재 디렉토리 모든 파일에서 문자열 검색

```
grep "STR" *
```

**파일 이름에 `*` 문자를 사용**하여, **현재 디렉토리에 있는 모든 파일에서 문자열을 검색**할 수 있다. 

**현재 디렉토리에 포함된 하위 디렉토리에 있는 파일은 탐색하지 않는다.** 

_하위 디렉토리를 탐색하려면 -r 옵션 사용_

```
$ ls
FILE1.txt  FILE2.txt
$ grep "PAT" *
FILE1.txt:grep searches for PATTERNS in each FILE.
FILE1.txt:PATTERNS is one or patterns separated by newline characters.
FILE2.txt:grep searches for PATTERNS in each FILE.
FILE2.txt:PATTERNS is one or patterns separated by newline characters.
```


## 특정 확장자를 가진 모든 파일에서 문자열 검색	


```
grep "STR" *.ext
```

파일 이름 **확장자 앞에 `*` 문자를 사용**하여, **특정 확장자를 가진 모든 파일에서 문자열을 검색**할 수 있다.


```
$ ls
A.c  A.h  B.c  B.h 
$ grep "include" *.h
A.h:#include <stdio.h>
B.h:#include <string.h"
```

<br>


## 대소문자 구분하지 않고 문자열 검색	

```
grep -i "STR" [FILE]
```

**`-i 옵션`을 사용**하여, **대소문자 구분없이 문자열을 검색**할 수 있다.

```
$ cat FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
And grep prints each line that matches a pattern.
$ grep -i "Pat" FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
And grep prints each line that matches a pattern.
```

<br>


## 매칭되는 PATTERN이 존재하지 않는 라인 선택	

```
grep -v "STR" [FILE]
```

문자열이 매칭되는 라인이 아닌, **매칭되는 패턴이 존재하지 않는 라인을 선택해야 하는 경우**가 있다. 

이 때, **`-v 옵션`을 사용**한다.

```
$ cat FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
And grep prints each line that matches a pattern.
$ grep -v "PAT" FILE1.txt
And grep prints each line that matches a pattern.
```

<br>


## 단어(Word) 단위로 문자열 검색	

```
grep -w "STR" [FILE]
```

**`-w 옵션`을 사용**하면, **단어(Word) 단위로 문자열을 검색**할 수 있다.

```
$ cat FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
And grep prints each line that matches a pattern.
$ grep -w "PAT" FILE1.txt
$ grep -w "PATTERNS" FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
```


<br>


## 검색된 문자열이 포함된 라인 번호 출력	

```
grep -n "STR" [FILE]
```

**`-n 옵션`을 사용**하여, **검색 결과가 포함된 라인 번호를 출력**할 수 있다.

```
$ cat FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
And grep prints each line that matches a pattern.
$ grep -n "PAT" FILE1.txt
1:grep searches for PATTERNS in each FILE.
2:PATTERNS is one or patterns separated by newline characters.
```

<br>


## 하위 디렉토리를 포함한 모든 파일에서 문자열 검색	

```
grep -r "STR" *
```

**`-r 옵션`을 사용**하면, **하위 디렉토리를 포함한 모든 파일에서 문자열을 검색**할 수 있다.

```
$ grep -r "PAT" *
DIR_1/FILE1.txt:grep searches for PATTERNS in each FILE.
DIR_1/FILE1.txt:PATTERNS is one or patterns separated by newline characters.
FILE1.txt:grep searches for PATTERNS in each FILE.
FILE1.txt:PATTERNS is one or patterns separated by newline characters.
FILE2.txt:grep searches for PATTERNS in each FILE.
FILE2.txt:PATTERNS is one or patterns separated by newline characters.
```

<br>


## 최대 검색 결과 갯수 제한	

```
grep -m 100 "STR" FILE
```

`grep` 명령 결과가 너무 많이 표시될 때, **`-m 옵션`을 사용**하여 **최대 표시 결과를 제한**할 수 있다.

```
$ cat FILE1.txt
grep searches for PATTERNS in each FILE.
PATTERNS is one or patterns separated by newline characters.
And grep prints each line that matches a pattern.
$ grep -n "PAT" FILE1.txt
1:grep searches for PATTERNS in each FILE.
2:PATTERNS is one or patterns separated by newline characters.
$ grep -m 1 "PAT" FILE1.txt
grep searches for PATTERNS in each FILE.
```

<br>




## 검색 결과 앞에 파일 이름 표시	

```
grep -H "STR" *
```

**`-H 옵션`을 사용**하여 **검색 결과 앞에 파일 이름을 표시**할 수 있다.

**`-Hn 옵션`**은 **파일 이름과 함께 라인 번호도 표시**한다.

```
$ grep -H "PAT" *
FILE1.txt:grep searches for PATTERNS in each FILE.
FILE1.txt:PATTERNS is one or patterns separated by newline characters.
$ grep -Hn "PAT" *
FILE1.txt:1:grep searches for PATTERNS in each FILE.
FILE1.txt:2:PATTERNS is one or patterns separated by newline characters.
```


<br>



## 문자열 A로 시작하여 문자열 B로 끝나는 패턴 찾기	

```
grep "A.*B" *
```

**정규 표현식에서 `.`와 `*`를 조합**하여 **문자열 A로 시작하여 문자열 B로 끝나는 패턴 검색**할 수 있다.

```
$ cat FILE1.txt
the first step  : edit text file.
the second step : save the file.
the third step  : copy to usb.
$ grep -n "the.*step" FILE1.txt
1:the first step  : edit text file.
2:the second step : save the file.
3:the third step  : copy to usb.
$ cat FILE2.txt
ABCDEFGHIJKLMNOPQRSTUVWXYZ
BCD
XYZ
ABZ
$ grep -n "A.*Z" FILE2.txt
1:ABCDEFGHIJKLMNOPQRSTUVWXYZ
4:ABZ
```

<br>



## 0-9 사이 숫자만 변경되는 패턴 찾기	

```
grep "STR[0-9]" *
```

**정규 표현식 `[]`를 사용**하여 **0-9 사이 숫자만 변경되는 문자열 패턴을 검색**할 수 있다.

```
$ cat FILE3.txt
step0  : edit text file.
step1  : save the file.
step2  : copy to usb.
$ grep -n step[0-9] FILE3.txt
1:step0  : edit text file.
2:step1  : save the file.
3:step2  : copy to usb.
```



<br>


## 문자열 패턴 전체를 정규 표현식 메타 문자가 아닌 일반 문자로 검색하기	

```
grep -F "*[]?..." [FILE]
```

**`-F 옵션`을 사용**하면, **패턴에 지정된 문자열을 메타 문자로 인식하지 않고 일반 문자로 인식하여 패턴을 검색**한다.

```
$ cat FILE4.txt
01234567890
[0-9]
12345
$ grep -n "[0-9]" FILE4.txt
1:01234567890
2:[0-9]
3:12345
$ grep -Fn "[0-9]" FILE4.txt
2:[0-9]
```

<br>


## 정규 표현식 메타 문자를 일반 문자로 검색하기	

```
grep "\*" [FILE]
```

문자열 패턴에서 **정규 표현식 메타 문자 앞에 "\"(백슬래시)를 사용**하면, **해당 문자를 일반 문자로 인식**하게 만들 수 있다.

```
$ cat FILE5.txt
* step 1
1. sample text 1
2. sample text 2
$ grep "\*" FILE5.txt
* step 1
$ grep "." FILE5.txt
* step 1
1. sample text 1
2. sample text 2
$ grep "\." FILE5.txt
1. sample text 1
2. sample text 2
```

<br>


## 문자열 라인 처음 시작 패턴 검색하기	

```
grep "^STR" [FILE]
```

**정규 표현식 `^`를 사용**하여 **문자열 라인의 중간이 아닌 시작 패턴만 검색**할 수 있다.

```
$ cat FILE5.txt
* step 1
1. sample text 1
2. sample text 2
$ grep "^1" FILE5.txt
1. sample text 1
```

<br>

## 문자열 라인 마지막 종료 패턴 검색하기	

```
grep "$STR" [FILE]
```

**정규 표현식 `$`를 사용**하여 **문자열 라인의 종료 패턴을 검색**할 수 있다.

```
$ cat FILE6.txt
..............
.............?
ABCDEFGHIJKLM.
$ grep "\.$" FILE6.txt
..............
ABCDEFGHIJKLM.
$ grep -v "\.$" FILE6.txt
.............?
```