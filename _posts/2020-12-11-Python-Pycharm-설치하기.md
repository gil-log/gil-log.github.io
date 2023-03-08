---
title: "[Python] Anaconda + Pycharm 연동"
last_modified_at: 2020-12-11T01:48
categories: 
  - python
tags: 
  - 'anaconda' 
  - 'pycharm' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[파이썬3 시작하기!(python 버전 선택과 디버깅 툴의 선택)[봉자씨의 즐퇴놀이]](https://bongjacy.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC3-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0python-%EB%B2%84%EC%A0%84-%EC%84%A0%ED%83%9D%EA%B3%BC-%EB%94%94%EB%B2%84%EA%B9%85-%ED%88%B4%EC%9D%98-%EC%84%A0%ED%83%9D)

[파이썬 코드 에디터 - 개발툴(IDE) 추천 및 다운로드 방법[scikitlearn]](https://scikitlearn.tistory.com/45)

[[AI/ML]Anaconda(아나콘다) 란??[kim-dragon]](https://kim-dragon.tistory.com/19)

[아나콘다(Anaconda) 설치 [bradbury]](https://bradbury.tistory.com/60?category=830131)

[Pycharm 설치하기(Feat. Anaconda) 및 환경설정[안홍자가 혼자 공부하면서 써보는 블로그]](https://hhahn.tistory.com/7)


# Anaconda

**`Anaconda`는 수학, 과학 분야에서 사용되는 여러 패키지를 묶어 놓은 Python 배포판**이다. 

**Python 기반의 데이터 분석에 필요한(각종 수학/과학 라이브러리들) 오픈소스를 모아놓은 개발 플랫폼**으로,

 **가상 개발 환경을 설정**하여 **각 프로젝트 별 개발 환경을 다르게 사용**할 수 있다.
_**Data Science와 Machine Learning 분야에서 Python을 사용하기 위해 기본적으로 설치하는 배포판이 되었다.**_

**`Anaconda`를 베이스**로 **Python을 활용**하여 **인공지능이나 데이터 분석을 시행하는 것이 정석화** 되었다.

<br>

**`Anaconda`**는 실제로 **`conda`, `Python` 및 150 개가 넘는 과학 패키지와 그 종속성과 함께 제공되는 소프트웨어 배포**이다. 
_Python에서 가장 일반적인 데이터 과학 패키지가 포함되어 있으므로 500MB정도의 큰 용량을 갖고 있다._



![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDMDYs%2FbtqAm3A8BJk%2FhIX0VJZuYEh9eo0xG263Qk%2Fimg.png)

`Anaconda`는 위 그림처럼 크게 네 부분으로 나뉜다. 

_`Anaconda Navigator`, `Anaconda Project`, `Data Science Libraries`, `Conda`_ 

**`Data Science Libraries`**는 **`Jupyter`와 같은 IDE 개발도구**와 `Numpy`, `SciPy` 같은 **과학 분석용 Libarary**, `Matplotlib` 같은 **데이터 시각화(Data Visualization) Libarary**, TensorFlow 같은 **Machine Learning Libarary 등을 포함**하고 있다. 

응용 프로그램 **`conda`는 패키지 및 가상 환경 관리자**이다. 
_다른 가상 환경 관리자 인 virtualenv 및 pyenv와 비슷한 역할_

## Why Anaconda Needed????

1. **`Anaconda`는 수많은 데이터 과학 패키지를 제공**하기 때문에 데이터 분석, 처리 작업을 시작할 수 있다. 

2. **`conda`를 사용하여 패키지와 환경을 관리**하면 **다양한 라이브러리를 다룰때 일어날 수 있는 문제를 줄일 수 있다.**

3. 컴퓨터 1대에서 **여러 프로젝트를 진행할 경우 환경이 꼬이는 것을 방지** 해준다.


## Anaconda 설치


### Python vs Anaconda

**`Anaconda`는 `Python`을 포함**하고 있기 때문에 **`Python`과 `Anaconda`를 둘다 설치하면 중복 파일이나 환경 변수 충돌 문제가 발생할 수도** 있다.

**`Python`은 pip툴만**을 가지고 있어 **`Anaconda`에 포함된 여러 라이브러리를 사용할 경우 일일이 설치**해줘야 한다.

따라서 **`Python`이 이미 설치된 상태로 `Anaconda`를 다운받는 경우**, **`Python`을 삭제하는 것**이 **나중에 발생할 문제를 방지**하는데 좋다.
_😨맙소사...😂_




### 기존 Python 삭제 하기

Windows 10 을 기준으로 설명하면 `윈도우 탭`에서 `설정`으로 들어간 후 `앱` 을 클릭한다.

![](https://images.velog.io/images/gillog/post/a783def0-850a-4b1a-8c26-35c7b8e0f093/image.png)

![](https://images.velog.io/images/gillog/post/b92edda3-51b7-48e5-89c7-4f1e9cdc767d/image.png)


앱 및 기능 에서 `Python`을 검색 한 후 모두 `제거` 해준다.

![](https://images.velog.io/images/gillog/post/9f35ec1b-46f2-4f7c-aeaa-2d97c8b3aab5/image.png)



이제 `Anaconda`를 설치하기 위해서는 [Anaconda Site - products - individual](https://www.anaconda.com/products/individual)에서 자신의 OS에 맞는 프로그램을 다운받아 설치한다.

![](https://images.velog.io/images/gillog/post/d236854a-8a10-4f4e-a49e-1abcc79b48d4/image.png)


Installer를 다운로드 받은 후 실행 시키고 next를 누르다 `Register Anaconda as my default Python` 버튼에 체크가 되어있는걸 확인하고 Install을 실행한다.
_`Register Anaconda as my default Python` : Anaconda를 기본 Python으로 등록할 지 여부 선택, 개발도구 및 에디터에서 Anaconda를 Python으로 인식_

_`Add Anaconda to my PATH environment variable` : 체크 안 할 경우 명령 프롬프트 또는 파워쉘에서 Python을 실행하려면 환경변수를 설정해줘야한다.
Python IDLE와 같은 통합개발환경에서만 사용할 경우 체크 안해도 됨_



![](https://images.velog.io/images/gillog/post/d3c543e5-55e2-4af3-8e94-180ff9ea3649/image.png)


![](https://images.velog.io/images/gillog/post/c4ad0269-5965-4370-908a-ff9e04886d04/image.png)

여기서 `Next`를 누르고 나면 설치가 `Finish`되었다.

`Anaconda`가 잘 설치되어있는지 확인해보려면, `Anaconda Prompt`를 실행 시킨 후 `python`을 명령어 창에 입력해보면 된다.


![](https://images.velog.io/images/gillog/post/372858c1-e78f-4d0b-877f-1f2625f7f189/image.png)

![](https://images.velog.io/images/gillog/post/69687d68-ce69-4333-9609-0780bc6e2d8d/image.png)


`python` 명령어를 입력해보면 설치된 python 버전과 해석기가 정상적으로 실행 된다.


![](https://images.velog.io/images/gillog/post/afe31cba-eea8-4ca9-a4a6-499b0fe45fbe/image.png)

또한 `Jupyter Notebook`과 `Spyder` 라는 Python IDE 들도 같이 설치가 되었다.

![](https://images.velog.io/images/gillog/post/4c3d01c6-720a-4a22-961b-50dcaae69015/image.png)

![](https://images.velog.io/images/gillog/post/d790c3a3-52b5-4f5b-bac2-413fab369028/image.png)


![](https://images.velog.io/images/gillog/post/cc9d5759-0f84-4bd7-b386-f9ce4147d35b/image.png)

이로써 `Anaconda` 설치가 완료 되었다.

 ---

# Pycharm

`Pycharm`은 Python 개발 툴 IDE 중 하나이다.

Python의 IDE로는 `VSCode`, `Anaconda`, `Jupyter Notebook`, `Pycharm` 등이 있다.

`Jupyter Notebook`은 간단한 Python 실습이나 데이터 분석용에서 사용방법도 간단하고 깔끔하게 코딩도 가능하다. 

Python 프로그램을 개발하는 경우엔 `PyCharm`을 사용하는 것이 권장된다.



## Pycharm 추천 이유

1. PyCharm은 무료
유료 버전도 존재하지만 무료 버전으로도 충분하다.

2. 사용하면서 다른 Python 버전을 선택하여 사용할 수 있다.

3. 패키지 설치가 간단하다. 
패키지가 설치 되어 있지 않다면 빨간줄이 쳐지는데 거기서 쉽게 설치가 가능하다.
_일반적으론 PIP를 사용하지만 그보다 더 쉽다_

4. 코드 실행이 간단하다. 
전체 프로그램을 실행하거나 간단한 테스트도 할 수 있다.

5. 파일 관리가 쉽고 함수 역시 보기 쉽게 정리가 된다.

5. 디버깅 기능이 좋으며, Pandas 데이터 프레임도 엑셀처럼 보여줄 수 있다.

---


# Pycharm 설치

JetBrains의 [pycharm](https://www.jetbrains.com/pycharm/)에서 PyCharm 설치 파일을 다운로드 해준다.

Community 버전으로도 충분히 사용할 수 있기에 Community 버전을 선택 하였다.

![](https://images.velog.io/images/gillog/post/352c676a-b89c-41bb-9965-b3c3de0878df/image.png)


설치된 Installer 파일을 실행 시켜 준다.

![](https://images.velog.io/images/gillog/post/71c67420-4499-4ea0-b963-7f35f761a171/image.png)

`Add launchers dir to the PATH`를 체크해 주어 환경 변수도 자동으로 업데이트 설정 해주었다.

![](https://images.velog.io/images/gillog/post/94cfb580-f751-4e0b-9e62-9bfc4f7ff5a4/image.png)

이제 재부팅을 하고 나서 `Pycharm`을 실행 한후 가볍게 `OK`를 눌러준다.

![](https://images.velog.io/images/gillog/post/39447980-b381-4700-a1a7-0e946715f460/image.png)
`Pycharm`이 실행 된 후 `New Project`를 눌러준다.
![](https://images.velog.io/images/gillog/post/c4159655-5421-430f-9fca-9d07f647c080/image.png)

프로젝트 생성 화면에서 `Basce interpreter`를 `Anaconda`의 `python.exe`로 바꿔주어야 한다.

![](https://images.velog.io/images/gillog/post/f17cb435-5e91-4e87-afb2-11c5f0b96a94/image.png)

아래와 같이 설치한 `Ancaonda`안에 있는 `python.exe` 경로로 바꿔준다.

![](https://images.velog.io/images/gillog/post/e62512ba-1f41-4f29-a04b-6bd7c3f9ff1a/image.png)

이제 `create`를 선택하자.

![](https://images.velog.io/images/gillog/post/6c391323-87f3-4cfd-b75c-739725f5b8e5/image.png)

성공적으로 Project가 생성 되었다.

프로젝트의 우클릭을 누른 후 `New` > `Python File` 후 .py file을 하나 생성 해본다.

![](https://images.velog.io/images/gillog/post/defaef3f-94a8-4c66-9f79-06081ab1fee8/image.png)

성공적으로 `Pycharm` 설치가 완료 되었다.

