---
title: "SVN(Subversion) - 개념 및 명령어"
last_modified_at: 2020-11-04T22:19
categories: 
  - 형상관리
tags: 
  - 'svn'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# SVN(Subversion)

`SVN(SubVersion)`은 **여러명이서 작업하는 프로젝트의 버전관리**나 **각자 만든 소스의 통합과 같은 문제를 해결**하기 위해 **저장소를 만들어 그곳에 소스를 저장**해 **소스 중복이나 여러 문제를 해결**하기 위한 **형상관리/소스 관리 툴**이다.
_형상관리 : 소스의 변화를 끊임없이 관리하는 것_



**프로젝트 소스는 SVN 서버의 Trunk라는 곳에 위치**하는데, **자신의 Local 저장소에 Trunk의 소스를 다운 받아(Update) 수정 및 추가 후 다시 업로드(Commit)하는 방식**이다.


자신만의 소스를 **다른 개발자들과 떨어져서 작업하려면 Branch를 만들어 작업 후 자기자신만 접근, 개발하여 완성되면 Merge 기능을 사용하여 Trunk와 소스를 합치면 된다.**

`A`가 자신이 수정한 소스나 폴더를 Commit하면 `B`는 해당 소스를 Update하면 최신 소스를 받아올 수 있다.




# SVN 사용 목적

형상관리 툴을 사용하게 되면 **소스를 버전 별로 관리할 수 있어서 개발할 때 실수로 소스를 삭제하거나, 수정하기 이전으로 돌아가야되는 경우에 유용하게 사용**할 수 있다.

팀 프로젝트에서도 누가, 무엇을, 어떻게 수정했는지도 알 수 있기 때문에 **코드를 병합하거나 수정된 소스를 추적하는 데에도 사용** 된다.

단순히 사용 목적을 나열하자면 아래와 같다.

- Revision 별로 파일 백업이 가능하다.
_파일 복원이 가능해지고, 실수가 발생해도 수정이 빨리 이루어 질 수 있다._


- 개발 버전과 배포 버전을 섞이지 않고 쉽게 관리가 가능하다.
_소스코드 버전관리_

- 파일 이름 변경, 이동, 디렉토리 버전 관리도 지원이 가능하다.

- 여러 사용자가 동시에 커밋을 하더라도 충돌이 발생하지 않는다.
_프로젝트 협업 지원_

- 한번 커밋이 발생 할때 마다 revision이 올라간다.
_어느 revision이 어떤 파일을 commit했는지 확인 용이_

- commit시에 자동 로그 기능이 있다.
_작업 이력 관리_

- 서버와 클라이언트 양방향 데이터 전송으로 네트워크 소통량이 최소화된다.

- 접근이 가능한 개발자는 누구든 쉽게 수정이 가능하다.




# SVN 용어 & 명령어

## 용어

### Repository
**프로젝트 파일 및 변경 정보가 저장되는 장소**로 모든 프로젝트의 **프로그램 소스들은 이 저장소 안에 저장** 된다. 
**한 프로젝트 마다 하나의 저장소**가 필요하며, **네트워크를 통해서 여러 사람이 접근** 할 수 있고, **소스뿐만이 아니라 소스의 변경 사항도 모두 저장**된다.



### Trunk
**프로젝트에서 가장 중심이 되는 디렉토리**로, **개발 소스를 Commit 했을 때 개발 소스가 모이는 장소**이다.
_소스와 파일 포함_

**모든 개발 작업은 trunk 디렉토리에서 이루어지며**, trunk 디렉토리 아래에는 바로 소스들의 파일과 디렉토리가 들어가게 된다.



 ### Branch

**trunk에서 뻗어져 나온 나뭇가지**를 뜻하며, **프로젝트 내의 작은 프로젝트의 개념**이다.

**trunk 디렉토리에서 개발하다** 보면 **큰 프로젝트에서 또 다른 작은 분류로 빼서 따로 개발해야 할 경우**가 생기는데, 이때 프로젝트안의 작은 프로젝트를 생성하는 개념으로, **branches 디렉토리 안에 또 다른 디렉토리를 두어 그 안에서 개발**하게 된다.

**trunk에서 분리/복사한 소스로 버전별 배포판을 생성**하거나 **trunk와 별도로 운영환경을 위한 안정화된 소스 관리 목적으로 사용**된다.



### Tag

tag는 꼬리표라는 뜻으로, **버전 별로 소스코드를 따로 관리하는 공간**이다.
_버전 별로 태그를 붙여 tag 디렉토리 안에 보관하는 개념_

특정 시점의 상태 보존 목적으로 사용 장기적으로 버전 별로(1.0, 1.1 등) 소스 코드를 따로 저장하는 공간이다.
_특정 시점에서 프로젝트의 스냅샷을 찍어두는 것_


### Revision

수정된 버전이라는 의미로, 클라이언트가 **Repository에 새로운 파일, 수정 등을 commit할때 마다 번호가 하나씩 증가**한다.


### Head

**Repository에 저장된 최신 revision을 의미**한다.
누군가에 의한 가장 최근에 commit된 revision이다.

### Base

클라이언트가 **checkout, update 등의 명령을 통해 Repository로 부터 내려받은 revision을 의미**한다.

**이 revision을 가지고 클라이언트는 수정 및 commit**을 하게 된다.

**만약 Head와 Base가 다르다면, commit이 거부되고 update를 먼저 수행해야 commit이 가능해진다.**

_내가 update한 후 소스 코드를 수정하고 있는 상황에서 다른 사람이 commit하여 revision이 증가한 경우 Head는 다른 사람이 commit한 revision이 되어 내 소스코드가 이전 revison이 된 경우이다._



---

## 명령어





### Import

```
svn import exampledir svn+ssh://svn-domain/svn/example/trunk
```

맨 처음 프로젝트 시작할때 빈 Repository에 **맨 처음 파일들을 저장소에 등록하는 명령어**이다.

### Checkout

```
svn checkout svn+ssh://svn-domain/svn/example/trunk example
```

**저장소에서 소스를 받아 오는 명령어**로, 받아온 소스에는 **소스 뿐만이 아니라 버전관리를 위한 파일도 같이 받아 온다.**
_지우거나 변경시 저장소와 연결 불가능_



### Export 

```
svn export svn+ssh://svn-domain/svn/example/trunk example
```

**Checkout과 달리 버전관리 파일을 뺀 순수한 소스만 가져오는 명령어**로 마지막에 사용한다.



### Commit
```
svn commit
```

로컬 저장소의 체크아웃 한 **소스의 변경된 내용(수정, 파일 추가, 삭제 등)을 저장소에 저장하여 갱신 하는 명령어**이다.
_Revision이 1 증가_


### Update

```
svn update
```
체크아웃 해서 받은 소스를 **서버 저장소의 최신 소스 버전으로 업데이트 하는 명령어**이다. 
_소스 수정이나 Commit 하기전에 한 번씩 해주는게 좋다._


### Revert

```
#로컬 저장소 복사본 exampleSource.java에 행했던 변경들을 모두 취소
svn revert exampleSource.java
```

**로컬 저장소의 소스 코드 내용을 이전 상태로 돌리는 명령어**이다.



### Log

```
svn log

#리비전 4의 변경사항 로그 보기
svn log -r 4

#리비전 4의 test.c 파일의 변경사항 로그 보기
svn log -r 4 test.java

#리비전 4 ~ 5의 변경사항 로그 보기
svn log -r 4:5
```

저장소에 **어떠한 것들이 변경 되었는지 revison을 통해 확인 할 수 있는 명령어**이다.



### Diff

```
svn diff[di] 

#저장소의 내용과 현재 작업 내용 중 main.java 파일이 차이를 확인
svn diff[di] main.java

#리비전 1과 2의 차이를 확인
svn diff[di] -r 1:2

#리비전 1과 현재 작업중인 main.java의 차이를 확인
svn diff[di] -r 1 main.java

#리비전 2와 현재 작업중인 디렉토리의 파일내용 차이를 확인
svn diff[di] -r 2
```
**예전 소스 파일과 지금의 소스 파일의 차이점을 비교해 보는 명령어**이다.


### Blame

```
svn blame[praise, annotate, ann] test.java
svn blame[praise, annotate, ann] -r 4 test.java
```

한 **소스 파일을 대상으로 각 리비전에 대해서 어떤 행을 누가 수정했는지 작업한 내용을 확인하는 명령어**이다.

출력 순서는 리비전, 커밋한 사용자의 ID, 소스 순이다.


### lock

```
svn lock hello.java
```

파일에 락을 걸어 락을 건 사용자만이 수정할 수 있게 해주는 명령어이다. 
해제는 `svn unlock`을 통해 할 수 있으며, 왜 파일에 락을 걸었는지도 로그를 기록 할 수 있다.


### Add

```
svn add hello.java
```

새 파일을 만들었을 경우에 파일을 추가 해주는 명령어이다. 
저장소에 저장은 되지 않아 **`add` 뒤엔 꼭 `svn commit`를 꼭 해줘야 한다.**
_add 후 commit을 안해주면 나중에 commit을 해도 안 올라간다._




### Status 
```
svn status
```

자신이 **수정하고 있는 파일의 상태를 알려주는 명령어**이다.





### Mkdir
```
svn mkdir newdir
```
**새로운 디렉토리를 만드는 명령어로 실제 변경사항은 `commit`시에 적용**된다.





### Delete

```
svn delete[del, rm, remove] newfile.java
```

**파일/디렉토리를 삭제하는 명령어**이다.



### Move
```
svn move[mv] test.java /src/
```
**파일을 이동하는 명령어**로 실제 변경사항은 **`commit`시에 적용**된다.


### rename

```
svn rename[ren] test.c sample.c
```

**파일 이름을 변경하는 명령어**로 실제 변경사항은 **`commit`시에 적용**된다.



### List

```
svn list[ls]
svn list[ls] svn://127.0.0.1/TestRepo1/trunk
```

**파일 리스트를 확인하는 명령어**이다.


### Switch

```
svn switch[sw] --relocate [이전주소] [새로운주소]
```

**소스 서버를 변경하는 명령어**이다.



### Info
```

svn info

# 로컬 저장소 정보 확인
svn info /svn_repos/LocalRepo1

# 원격 저장소 정보 확인
svn info svn://127.0.0.1/TestRepo1

```

**로컬 저장소 또는 원격 저장소의 파일, 폴더 정보를 확인하는 명령어**이다.










# SVN vs GIT

### SVN

 - `SVN`은 대부분의 기능을 완성해놓고 소스를 중앙 저장소에 commit한다.
_SVN에서 commit은 중앙 저장소에 해당 기능을 공개한다는 의미._

 - **GIT 과 가장 큰 차이점**은 **개발자가 자신만의 Version History를 가질 수 없다. **
_local History를 이용하긴 하지만 일시적이고, 자신이 몇일전 까지에 한해 작업한 내역을 확인 가능하지만 버전 관리가 되지는 않는다._

 - **Commit한 내용에 실수가 있을 시에 다른 개발자에게 바로 영향**을 미치게 된다.



### GIT

 - `GIT`은 **개발자가 자신만의 Commit History**를 가질 수 있고, **개발자 저장소와 서버 저장소를 독립적으로 관리**가 가능하다.
_매우 유연한 방식으로 소스를 운영할 수 있으며, 이러한 유연성이 git의 가장 큰 장점_

 - ** Commit한 내용에 실수가 있더라도 바로 서버에 영향을 미치지 않는다.**

 - 마음대로 Commit(Push)하다가 자신이 원하는 순간에 서버에 변경 내역(Commit History)을 보낼 수 있으며, 서버의 통합 관리자는 관리자가 원하는 순간에 각 개발자의 Commit History를 가져올 수 있다.





<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[SVN] SVN 사용 방법, 다른 사람의 SVN Project를 사용해보자[쩨리쩨리]](https://jerryjerryjerry.tistory.com/55)

[Subversion(SVN) 용어 및 사용법 Gyrfalcon[Minsub's Blog]](https://gyrfalcon.tistory.com/entry/SubversionSVN-%EC%9A%A9%EC%96%B4-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95)

[[웹개발 기초] 형상관리툴이란? (SVN GIT 간단비교)[갓대희의 작은공간]](https://goddaehee.tistory.com/158)

[SVN 이란? SVN 사용 이유](https://na27.tistory.com/211)

[Subversion(SVN) 개념 및 명령어 정리[갓우리코딩]](https://hellowoori.tistory.com/57)

[[SVN] revision / BASE / HEAD의 개념[solder1819]](https://m.blog.naver.com/PostView.nhn?blogId=solder1819&logNo=80201677200&proxyReferer=https:%2F%2Fwww.google.com%2F)