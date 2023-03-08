---
title: "[MacOS] MySQL 설치하기"
last_modified_at: 2021-06-03T22:51
categories: 
  - macos
tags: 
  - 'MacOS' 
  - 'mysql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[macOS MySQL 설치 및 설정 사용법[WHITEPAEK Tech Docs]](https://whitepaek.tistory.com/16)

[]()

[]()

[]()

[]()

[]()

<br>

이번엔 **MacOS인 내 MackBook Air에 MySQL을 설치**하려 한다.

**macOS용 Package 관리자 `Homebrew`로 설치**하면 **MySQL 사이트에 접속해서 설치하는 것 보다 훨씬 간편하게 설치 할 수 있다**.
_역시 MacOS 👍🏻_

# 설치과정

먼저 **터미널을 실행**시키고 **`brew update` 명령어로 `Homebrew`를 최신 상태로 업데이트** 해준다.

![](https://images.velog.io/images/gillog/post/cc6e338a-2bba-47fa-9677-572e453c8db1/image.png)

그 다음, **`brew search mysql` 명령어로 설치 가능한 mysql 관련 package를 검색**한다.

![](https://images.velog.io/images/gillog/post/158a2d62-31a5-4582-a06e-b7d30afd6f7a/image.png)

이후 **MySQL 최신 버전을 설치하고 싶다면, `brew install mysql`**을, 
**특정 버전을 설치하고 싶다면 `brew install mysql@5.7` 과 같이 버전을 명시**해주면 된다.

나는 MySQL 5.X 버전들만 사용해보아서 **이번엔 최신 버전을 한번 사용해보고 싶어 `brew install mysql`로 mysql을 설치**했다.

![](https://images.velog.io/images/gillog/post/e86ff250-3766-4093-8602-d7dc48f67be9/image.png)

역시 터미널로 설치하면 참 쉽다~~ 굳! 👏🏻

---
# 최초 설정

이제 **MySQL도 설치했으니 기본 설정을 진행**해보자!

**`mysql.server start` 명령어로 MySQL 서버를 실행**한다.

![](https://images.velog.io/images/gillog/post/5ebba7c6-7d8a-4b25-bd7a-0d829df1aa12/image.png)

**설정을 위해, `mysql_secure_installation` 명령어를 입력**한다.

![](https://images.velog.io/images/gillog/post/5c0ce896-3272-4dbe-9a86-5bd216f5712a/image.png)

위 문구 처럼 **비밀번호 설정에 관한 옵션**이 나오는데, 
**Y를 입력하면 복잡한 비밀번호를 설정해야 하고, 
그 외에 다른 키(N)을 입력하면 단순한 비밀번호 사용이 가능**하다.

난 No~를 입력했다.

![](https://images.velog.io/images/gillog/post/773bf2f1-0846-4b06-9206-166e499e3a4e/image.png)

그 후 새로운 비밀번호를 입력하고 나면, 
**anonymous users를 지우겠냐는 설정**이 나온다.

**Y를 선택 할 경우, `mysql -uroot` 처럼 `-u` 옵션이 필요**하고,
**N을 선택 할 경우, `mysql`과 같이 `-u` 옵션이 필요 없게 된다.**

나는 Yes를 선택 했다.

![](https://images.velog.io/images/gillog/post/550014d3-e44d-4af8-9908-34f566e0a363/image.png)

다음은 **원격 IP에서 Root 계정에 대한 원격 접속을 허용할지에 대한 옵션**이다.

**Y를 선택 할 경우, 원격 접속 불가능
N을 선택 할 경우, 원격 접속이 가능**해진다.

나는 N을 선택!

![](https://images.velog.io/images/gillog/post/d7db251e-fe28-468a-bc7c-fa083dfa33d6/image.png)

다음은 기본 **Test DB를 제거할지에 대한 설정**이다.

**나는 Y! Test DB를 제거**했다.

![](https://images.velog.io/images/gillog/post/b342db93-90e1-4dfc-9fb1-e19196a1444a/image.png)

다음은 **변경된 권한을 테이블에 적용하는 설정인데, 이건 무조건 Y를 해야 한다.**



![](https://images.velog.io/images/gillog/post/fe4f73e4-96df-4eba-941d-a20e22506c67/image.png)

이제 설정은 끝났다!
_Yes~~~ All Done!_



마지막 확인을 위해 **`mysql -uroot -p`로 아까전 설정한 새 비밀번호로 로그인** 해보자!

![](https://images.velog.io/images/gillog/post/b8448040-d59a-46ab-b62b-b0e98339859b/image.png)

Yes~~~

이제 **`mysql>` shell로 변경된 상태**에서 **`status` 명령어로 characterest가 모두 utf8로 설정되어있는지 확인**하자!

![](https://images.velog.io/images/gillog/post/7c333fe4-805c-49eb-a995-5699e268e9d2/image.png)


**MySQL 서버를 종료하고 싶다면 `mysql.server stop`을 입력**하면 된다.

_설치 끝._