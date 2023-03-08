---
title: "[Python] 표준, Data Science Library 소개"
last_modified_at: 2020-12-17T06:39
categories: 
  - python
tags: 
  - 'python' 
  - '표준 Library'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[점프 투 파이썬](https://wikidocs.net/28)

[python 표준 라이브러리 소개[wooiljeong]](https://wooiljeong.github.io/python/python_std_library/)

[]()

[]()

<br>

---
# 표준 Library

---
## sys

**`sys` 모듈은 파이썬 인터프리터가 제공하는 변수와 함수를 직접 제어할 수 있게 해주는 모듈**이다.

### sys.argv

**`sys.argv`는 명령 행에서 인수를 전달해주는 함수**이다.

```python
# argv_test.py
import sys
print(sys.argv)
```

```
C:/User/home>python gillog.py gil log Hello
```
명령 프롬프트 창에서 위 예처럼 gillog.py 뒤에 또 다른 값을 함께 넣어 주면 sys.argv 리스트에 그 값이 추가된다.

```
C:/doit/Mymod>python gillog.py gil log Hello
['gillog.py', 'gil', 'log', 'Hello']
```
**python 명령어 뒤**의 **모든 것들이 공백을 기준으로 나뉘어서 sys.argv 리스트의 요소**가 된다.

_ **명령 프롬프트 창에서는 `/`, `\`든 상관없지만, 소스코드 안에서는 반드시 `/` 또는 `\\` 기호를 사용**해야 한다._

### sys.exit

`sys.exit`는 강제로 스크립트를 종료하는 함수이다.

```python
sys.exit()
```

**`sys.exit`는 `Ctrl+Z`나 `Ctrl+D`를 눌러서 대화형 인터프리터를 종료하는 것과 같은 기능**을 한다. 

**프로그램 파일 안에서 사용하면 프로그램을 중단**시킨다.

### sys.path

**`sys.path`는 파이썬 모듈들이 저장되어 있는 위치**를 나타낸다.

**즉 이 위치에 있는 파이썬 모듈은 경로에 상관없이 어디에서나 불러올 수 있다.**

```python
import sys
sys.path
['', 'C:\\Windows\\SYSTEM32\\python37.zip', 'c:\\Python37\\DLLs', 
'c:\\Python37\\lib', 'c:\\Python37', 'c:\\Python37\\lib\\site-packages']
```

**위 예에서 `''`는 현재 디렉터리**를 말한다.

```python
import sys
sys.path.append("C:/doit/mymod")
```
**위와 같이 파이썬 프로그램 파일에서 sys.path.append를 사용해 경로 이름을 추가할 수 있다.** 

이렇게 하고 난 후에는 **C:/doit/Mymod 디렉터리에 있는 파이썬 모듈을 불러와서 사용할 수 있다.**


---

## pickle

**`pickle`은 객체의 형태를 그대로 유지하면서 파일에 저장하고 불러올 수 있게 하는 모듈**이다. 


### pickle.dump

`pickle.dump()`는 딕셔너리 객체인 data를 그대로 파일에 저장하는 함수 이다.

```python
import pickle
f = open("gillog.txt", 'wb')
data = {1: 'gil', 2: 'log'}
pickle.dump(data, f)
f.close()
```

### pickle.load

`pickle.load`는 원래 있던 딕셔너리 객체(data) 상태 그대로 불러오는 함수이다.

```python
import pickle
f = open("gillog.txt", 'rb')
data = pickle.load(f)
print(data)
{2:'log', 1:'gil'}
```

---

## os

**`os`는 환경 변수나 디렉터리, 파일 등의 OS 자원을 제어할 수 있게 해주는 모듈**이다.

### os.environ

`os.environ`는 현재 자신의 시스템의 환경 변수 값을 알고 싶을때 사용하는 함수이다.

```python
import os

os.environ
environ({'PROGRAMFILES': 'C:\\Program Files', 'APPDATA': … 생략 …})
```
**`os.environ`은 환경 변수에 대한 정보를 딕셔너리 객체로 돌려준다.**


### os.chdir

`os.chdir`는 현재 디렉터리 위치를 변경할 수 있는 함수이다.

```python
import os

os.chdir("C:\WINDOWS")
```

### os.getcwd

`os.getcwd`는 현재 자신의 디렉터리 위치를 돌려준다.

```python
import os

os.getcwd()

#C:\Users\ProfessorG\PycharmProjects\GilPyBrain\python\library
```

### os.system

**`os.system`는 시스템 자체의 프로그램이나 기타 명령어를 파이썬에서 호출해주는 함수**이다.

**`os.system("명령어")`처럼 사용**한다.

```python
import os

os.system("dir")
```


### os.popen

**`os.popen`는 시스템 명령어를 실행한 결과값을 읽기 모드 형태 파일 객체로 돌려주는 함수**이다.

```python
import os

f = os.popen("dir")

print(f.read())

'''
 C 드라이브의 볼륨에는 이름이 없습니다.
 볼륨 일련 번호: xxxx-xxxx

 C:\Users\ProfessorG\PycharmProjects\GilPyBrain\python\library 디렉터리

2020-12-17  오후 03:11    <DIR>          .
2020-12-17  오후 03:11    <DIR>          ..
2020-12-17  오후 03:11                75 basic_library.py
2020-12-17  오후 03:08                 0 __init__.py
               2개 파일                  75 바이트
               2개 디렉터리  42,355,130,368 바이트 남음

'''
```


### ETC os function


|함수	|설명|
|:--:|:--:|
|os.mkdir(디렉터리)|	디렉터리를 생성한다.|
|os.rmdir(디렉터리)	|디렉터리를 삭제한다.단, 디렉터리가 비어있어야 삭제가 가능하다.|
|os.unlink(파일)	|파일을 지운다.|
|os.rename(src, dst)|	src라는 이름의 파일을 dst라는 이름으로 바꾼다.|


---

## shutil

shutil은 파일을 복사해 주는 파이썬 모듈이다.

```python
import shutil
shutil.copy("src.txt", "dst.txt")
```
src라는 이름의 파일을 dst로 복사한다. 

만약 dst가 디렉터리 이름이라면 src라는 파일 이름으로 dst 디렉터리에 복사하고 동일한 파일 이름이 있을 경우에는 덮어쓴다.

---

## glob

**`glob`은 특정 디렉터리에 있는 파일 이름 모두를 알아야 할 때 사용하는 모듈**이다.

### glob(pathname)

`glob(pathname)`는 디렉터리에 있는 파일들을 리스트로 만들어 주는 함수이다.
_디렉터리 안의 파일들을 읽어서 돌려준다. `*`, `?` 등 메타 문자를 써서 원하는 파일만 읽어 들일 수도 있다._
```python
import glob

# * 이용 mark로 시작하는 파일들만 찾아 읽어들이기
glob.glob("c:/doit/mark*")
['c:/doit\\marks1.py', 'c:/doit\\marks2.py', 'c:/doit\\marks3.py']
```

---

## tempfile

**`tempfile`은 파일을 임시로 만들어서 사용할 때 유용한 모듈**이다.

### tempfile.mkstemp

**`tempfile.mkstemp()`는 중복되지 않는 임시 파일의 이름을 무작위로 만들어서 돌려주는 함수**이다.

```python
import tempfile

filename = tempfile.mkstemp()

print(filename)
# 'C:\WINDOWS\TEMP\~-275151-0'
```

### tempfile.TemporaryFile

**`tempfile.TemporaryFile()`은 임시 저장 공간으로 사용할 파일 객체를 돌려주는 함수**이다.

이 파일은 **기본적으로 바이너리 쓰기 모드(wb)**를 갖는다. 

**`f.close()`가 호출되면 이 파일 객체는 자동으로 사라진다.**

```python
import tempfile

f = tempfile.TemporaryFile()
f.close()
```

---

## time


### time.time

**`time.time()`**은 **UTC(Universal Time Coordinated 협정 세계 표준시)를 사용**하여 **현재 시간을 실수 형태로 돌려주는 함**수이다. 

1970년 1월 1일 0시 0분 0초를 기준으로 지난 시간을 초 단위로 돌려준다.

```python
import time

time.time()
988458015.73417199
```

### time.localtime

**`time.localtime()`은 `time.time()`이 돌려준 실수 값을 사용해서 연도, 월, 일, 시, 분, 초, ... 의 형태로 바꾸어 주는 함수**이다.

```python
import time

time.localtime(time.time())

time.struct_time(tm_year=2013, tm_mon=5, tm_mday=21, tm_hour=16,
    tm_min=48, tm_sec=42, tm_wday=1, tm_yday=141, tm_isdst=0)
    
```    

### time.asctime

**위 `time.localtime`에 의해서 반환된 튜플 형태의 값을 인수로 받아서 날짜와 시간을 알아보기 쉬운 형태로 돌려주는 함수**이다.

```python
...
time.asctime(time.localtime(time.time()))
'Sat Apr 28 20:50:20 2001'
```

### time.ctime

`time.asctime(time.localtime(time.time()))`은 **`time.ctime()`을 사용해 간편하게 표시**할 수 있다. 

**asctime과 다른 점**은 **ctime은 항상 현재 시간만을 돌려준다는 점**이다.

```python
...
time.ctime()
'Sat Apr 28 20:56:31 2001'
```

### time.strftime

**`strftime` 함수는 시간에 관계된 것을 세밀하게 표현하는 여러 가지 포맷 코드를 제공**한다.

```python
time.strftime('출력할 형식 포맷 코드', time.localtime(time.time()))
```

시간에 관계된 것을 표현하는 포맷 코드

|포맷코드	|설명|	예|
|:--:|:--:|:--:|
|%a|	요일 줄임말	|Mon|
|%A	|요일|	Monday|
|%b	|달 줄임말|	Jan|
|%B	|달	|January|
|%c	|날짜와 시간을 출력함|	06/01/01 17:22:21|
|%d	|날(day)	|[01,31]|
|%H	|시간(hour)-24시간 출력 형태	|[00,23]|
|%I	|시간(hour)-12시간 출력 형태|	[01,12]|
|%j	|1년 중 누적 날짜	|[001,366]|
|%m	|달	|[01,12]|
|%M	|분	|[01,59]|
|%p|	AM or PM	|AM|
|%S	|초	|[00,59]|
|%U|	1년 중 누적 주-일요일을 시작으로	|[00,53]|
|%w	|숫자로 된 요일|	[0(일요일),6]|
|%W	|1년 중 누적 주-월요일을 시작으로|	[00,53]|
|%x|	현재 설정된 로케일에 기반한 날짜 출력|	06/01/01|
|%X	|현재 설정된 로케일에 기반한 시간 출력|	17:22:21|
|%Y|	년도 출력|	2001|
|%Z	|시간대 출력	|대한민국 표준시|
|%%	|문자|	%|
|%y	|세기부분을 제외한 년도 출력|	01|

```python
import time

time.strftime('%x', time.localtime(time.time()))
'05/01/01'

time.strftime('%c', time.localtime(time.time()))
'05/01/01 17:22:21'
```


### time.sleep

**`time.sleep `함수는 주로 루프 안에서 많이 사용**한다. 

**이 함수를 사용하면 일정한 시간 간격을 두고 루프를 실행**할 수 있다.

```python
import time

for i in range(10):
    print(i)
    time.sleep(1)
```
**time.sleep 함수의 인수는 실수 형태를 쓸 수 있다. **
_즉 **1이면 1초, 0.5면 0.5초가 되는 것**_


---


## calendar

**`calendar`는 파이썬에서 달력을 볼 수 있게 해주는 모듈**이다.

### calendar.calendar(연도)

**`calendar.calendar(연도)`는 그 해의 전체 달력**을 볼 수 있다.
```python

import calendar

print(calendar.calendar(2015))
```

### calendar.prcal(연도)

`calendar.prcal(연도)`를 사용해도 위와 똑같은 결괏값을 얻을 수 있다.

```python
calendar.prcal(2015)


calendar.prmonth(2015, 12)

'''
December 2015
Mo Tu We Th Fr Sa Su
    1  2  3  4  5  6
7  8  9  10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31
'''
```

### calendar.weekday

**`weekday(연도, 월, 일)` 함수는 그 날짜에 해당하는 요일 정보를 돌려준다.** 

**월요일은 0, 화요일은 1, 수요일은 2, 목요일은 3, 금요일은 4, 토요일은 5, 일요일은 6이라는 값을 돌려준다.**


```python
calendar.weekday(2015, 12, 31)
3
# 2015년 12월 31일은 목요일
```


### calendar.monthrange

**`monthrange(연도, 월)` 함수는 입력받은 달의 1일이 무슨 요일인지와 그 달이 며칠까지 있는지를 튜플 형태로 돌려준다.**
```python
calendar.monthrange(2015,12)
(1, 31)
# 2015년 12월 1일은 화요일이고, 이 달은 31일까지 있다
```
---

## random
**random은 난수(규칙이 없는 임의의 수)를 발생시키는 모듈**이다.

### random.random()

`random.random()`은 0.0에서 1.0 사이의 실수 중에서 난수 값을 돌려주는 함수이다.

```python
import random

random.random()
0.53840103305098674
```
### random.randint(, )

**`random.randint(시작, 끝)`은 범위 사이의 정수 중에서 난수 값을 돌려주는 함수**이다.

```python
random.randint(1, 55)
43
```

### random.shuffle()

**`random.shuffle()`는 리스트의 항목을 무작위로 섞고 싶을 때 사용하는 함수**이다.

```python
import random

data = [1, 2, 3, 4, 5]
random.shuffle(data)

data
[5, 1, 3, 4, 2]
```


### random.choice()
**`random.choice()` 는 입력으로 받은 리스트에서 무작위로 하나를 선택하여 돌려준다.**

---


## webbrowser
**`webbrowser`는 자신의 시스템에서 사용하는 기본 웹 브라우저를 자동으로 실행하는 모듈**이다.

### webbrowser.open()

**`webbrowser.open()`는 웹 브라우저가 이미 실행된 상태라면 입력 주소로 이동**한다. 

**만약 웹 브라우저가 실행되지 않은 상태라면 새로 웹 브라우저를 실행한 후 해당 주소로 이동**한다.

```python
import webbrowser

webbrowser.open("http://google.com")
```


### webbrowser.open_new()

**`webbrowser.open_new()` 는 이미 웹 브라우저가 실행된 상태이더라도 새로운 창으로 해당 주소가 열리게 한다.**

```python
webbrowser.open_new("http://google.com")
```
---


<br>

---

# Data Science Library

아래는 **Data Science에서 사용하는 Python Library들의 목록**이다.

## 수치해석

 NumPy

scipy

sympy

## 데이터 수집

BeautifulSoup
- 크롤링(Crawling)

Selenium

Quandl

PublicDataReader

## 데이터 탐색 및 전처리
Pandas

pdpipe

modin

MDP

Orange

## 데이터 시각화

matplotlib

seaborn

plotly

folium

bokeh

## 이미지 처리

pilow

scikit-image

OpenCV

## 자연어 처리

KoNLP

NLTK

Gensim

## 음향신호 처리

PyAudio-Analysis

LibRosa

## 시계열 분석

Statsmodels

Filterpy

Hmmlearn

Prophet

## 머신러닝

Scikit-Learn

## 딥러닝 프레임워크
TensorFlow

PyTorch

Keras

## 확률 모델

LibPGM

Pgmpy

## 베이지안 모델

pyMC3