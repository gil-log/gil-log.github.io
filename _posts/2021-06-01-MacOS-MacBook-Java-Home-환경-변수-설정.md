---
title: "[MacOS] MacBook Java Home 환경 변수 설정"
last_modified_at: 2021-06-01T00:02
categories: 
  - macos
tags: 
  - 'Java' 
  - 'MacOS'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[맥 MacOS에 JAVA_HOME 환경설정[khstar]](https://khstar.tistory.com/entry/%EB%A7%A5-MacOS%EC%97%90-JAVAHOME-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95)

[]()

[]()

[]()

[]()

[]()

<br>

---

**`VSCode`에서 `Spring Boot`로 작업**할게 생겼는데, **JDK Java Home 설정**을 해줘야 하는 부분이 생겼다.

**`MacOS`에서 `JAVA HOME` Path 설정을 진행**해보려 한다.


---

먼저, **MacOS 에서 JDK 설치가 완료된 상태**에서, `java -version`으로 정상적으로 설치된 상태를 확인하면, **JDK 기본 설치 경로는 `/Library/Java/JavaVirtualMachines`**이다.

![](https://images.velog.io/images/gillog/post/aa93da0e-dedd-4f4c-94d7-70cc332ed200/image.png)


이제 사용자 `Home`으로 이동한다.

`cd ~/`

이동후 환경설정을 위해 vi로 `.bash_profile`을 만든다. 

`$vi .bash_profile`

![](https://images.velog.io/images/gillog/post/99f5cfbd-051f-478c-9add-1e1c5933919d/image.png)

그 후 아래 내용을 입력 후 저장한다.
_JAVA_HOME 경로는 각자 Java Version에 맞게 설정_

```bash
JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home
PATH=$PATH:$JAVA_HOME/bin
export JAVA_HOME
export PATH
```

![](https://images.velog.io/images/gillog/post/483b306e-42c5-4511-a298-1345320cf85c/image.png)

**vi 편집기 저장 후 `source` 명령어를 이용해 환경설정을 적용**한다. 

`source .bash_profile`

![](https://images.velog.io/images/gillog/post/0ecf09ed-3540-499a-af86-718b33378b84/image.png)


**환경 설정이 적용 되었는지 확인하기 위해 JAVA_HOME을 출력**해본다.

`echo $JAVA_HOME`


![](https://images.velog.io/images/gillog/post/7e313493-0e86-4c35-b597-d80e20b34301/image.png)