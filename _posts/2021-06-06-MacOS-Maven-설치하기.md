---
title: "[MacOS] Maven 설치하기"
last_modified_at: 2021-06-06T22:46
categories: 
  - macos
tags: 
  - 'MacOS' 
  - 'maven'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---


# 🙆‍♂️ import 🙇‍♂️

[[MAC]Maven 설치 및 환경설정(터미널 사용)[A green developer 햇님이]](https://es2sun.tistory.com/226)

[]()

<br>

---

MacOS에서 Maven 환경 설정을 진행 해보려한다.


# Maven 설치하기

[이곳](http://maven.apache.org/download.cgi)을 클릭해 **`apache.maven` 사이트에 접속**한다.

**`Binary tar.gz archive`에서 `apache-maven-version-bin.tar.gz` 파일을 설치**한다.

![](https://images.velog.io/images/gillog/post/d7c3c4c4-4840-48e5-9a80-5e81fe47fd2e/image.png)


압축 해제 후 자신이 설정하고 싶은 폴더 경로에 저장 폴더를 위치시킨다.

![](https://images.velog.io/images/gillog/post/2818cfd8-cb6e-4ea3-b45e-2d56bf53ee58/image.png)

이제 터미널에서 `bash_profile`을 건드려야 하는데, 
미리 **`pwd`로 압축을 해제한 `apache-maven` 경로를 복사**해주면 좋다.

![](https://images.velog.io/images/gillog/post/d906974d-75d9-4735-b5f8-06c3d7dead2c/image.png)

**`vi ~/.bash_profile`로 환경 변수를 추가**해주어야한다.

![](https://images.velog.io/images/gillog/post/d4e9de61-f40f-434e-ac5b-f16bc2182be6/image.png)

아래 처럼 **`M3`관련 내용을 추가하고 저장**하자.

![](https://images.velog.io/images/gillog/post/0d331994-d8d9-4f2e-babd-fce7cba17806/image.png)

**`source .bash_profile` 명령 후에 `mvn -version`으로 설치가 잘 진행 됬는지 확인**하자.

![](https://images.velog.io/images/gillog/post/c43b4dd3-c084-4c80-9564-6b0684ae960b/image.png)

`maven 설치 완료!`