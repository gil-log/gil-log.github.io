---
title: "[Linux] Logrotate"
last_modified_at: 2021-08-05T23:34
categories: 
  - linux
tags: 
  - 'Logrotate' 
  - 'linux'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Linux] Logrotate 설정[byd0105]](https://byd0105.tistory.com/29)

[리눅스 로그관리 - Logrotate 알아보기[서버구축이야기!]](https://server-talk.tistory.com/271)


---

# Logrotate

**`Logrotate`**는 **`Linux`에서 log를 저장하며 관리** 할때 **특정 log 파일이 한 파일로 계속**해서,

**크기가 커지며 저장되는 걸 분산시켜줄때 사용**한다.

**한 log 파일에 log가 지속적으로 쌓이게** 되면, 

**log 확인이 필요한 경우 너무 방대해 확인이 어려워** 지고,

**디스크 용량 또한 낭비**된다.


## Logrotate 실행 순서

![](https://images.velog.io/images/gillog/post/d78caa68-b80f-495e-9222-b0bb998f13d0/image.png)

**`Logrotate`는 위 사진과 같은 순서대로 동작**한다.


## Logrotate 구조


**`Logrotate`를 구성하는 파일들은 아래와 같은 구조**를 가진다.


- `/usr/sbin/logrotate`  : **Logrotate 데몬 프로그램**

- `/etc/logrotate.conf` : **Logrotate 데몬 설정 파일**

- `/etc/logrotate.d/` : **Logrotate 프로세스 설정 파일**

- `/etc/cron.daily/logrotate` : **Logrotate 작업내역 로그 **

## Logrotate 설치

**`Logrotate`는 `Linux` System에서 log 관리를 위해 사용**하는데, 

**OS 설치시 기본적으로 설치**되어 있다.

**`Logrotate`가 설치되어있는지 확인 하려면 아래 명령어를 입력**해보면 된다.

```cmd
rpm -qa | grep logrotate
```

![](https://images.velog.io/images/gillog/post/ae9447b8-6779-45bf-96aa-49693b0b937b/image.png)

**만약 설치되어 있지 않다면 아래 명령어로 설치**할 수 있다.

```cmd
yum -y install logrotate
```

---

# Logrotate 사용하기

**`Logrotate`를 사용하기 위해 아래 내용들을 먼저 확인하고 사용**하려한다.

## Option

- rotate [숫자] : log파일이 5개 이상 되면 삭제 
_ex) rotate 5_

- maxage [숫자] : log파일이 30일 이상 되면 삭제 
_ex) maxage 30_

- size : 지정된 용량보다 클 경우 rotate 실행 
_ex) size +100k_

- create [권한] [유저] [그룹] : rotate 되는 로그파일 권한 지정
_ex) create 644 root root_

- notifempty : 로그 내용이 없으면 rotate 하지 않음 

- ifempty : 로그 내용이 없어도 rotate 진행 

- monthly(월) , weekly(주) , daily(일) rotate 진행

- compress : 로테이트 되는 로그파일 gzip 압축

- nocompress : 로테이트 되는 로그파일 gzip 압축 X

- missingok : 로그 파일이 발견되지 않은 경우 에러처리 하지 않음

- dateext : 백업 파일의 이름에 날짜가 들어가도록 함  





## logrotate.conf 설정

**`logrotate.conf`는 `Logrotate` 실행의 모든 설정을 담당**한다.


**아래와 같은 내용을 설정**해주면 된다.

```
# rotate log files weekly
# log 회전 주기 yearly : 매년, monthly : 매월, weekly : 매주, daily : 매일
daily

# keep 4 weeks worth of backlogs
# log 파일 개수, 해당 개수가 넘어가면 logrotate의 주기에 따라 실행됨
rotate 7

# create new (empty) log files after rotating old ones
# 새로운 log 파일 생성 여부, create : log 파일 생성, empty : log 파일 생성 안함
create

# use date as a suffix of the rotated file
# 파일명 날짜 여부, logrotate 실행 후 log파일에 날짜를 부여
dateext

# uncomment this if you want your log files compressed
# log파일 압축 여부, 로그 파일 크기 조절 용도
# compress

# RPM packages drop log rotation information into this directory
# 개별 로그 process 설정 경로
include /etc/logrotate.d
```

![](https://images.velog.io/images/gillog/post/38e2fd2c-34bf-4866-82d6-12fd75e0b905/image.png)

## logrotate.d 설정

**`/etc/logrotate.d` 에서**는 **`Logrotate`를 실행하는 개별 프로세스들에 대한 설정을 지정**할 수 있다.


**먼저 `/etc/logrotate.d` 경로**에 들어가준다.

```cmd
cd /etc/logrotate.d
```
![](https://images.velog.io/images/gillog/post/86845c66-3e3c-4d13-8242-6759794e4166/image.png)

**해당 경로에서 log 생성을 원하는 config 파일을 작성**해주면 되는데,

**아래와 같은 형식**이다.

```cmd
/var/log/maillog /var/log/freshclam.log {
	// 일 단위로 실행
	daily
    	// 회전 주기 파일 개수
        rotate 7
        // log 파일 내용 없을 시 rotate 하지 않음
        notifempty
        // log 파일 없을 경우 error 메시지 출력 후 다음 실행
        missingok
        // 로그 파일 압축
        compress
        // 여러개 log 파일을 script로 공유하여 실행
        sharedscripts
        // logrotate 실행 후 스크립트 실행(스크립트 파일 경로가 와도 됨)
        postrotate
                /bin/kill -HUP `cat /var/run/syslogd.pid 2>/dev/null` 2> /dev/null || true
        endscript
}    
```

## Logrotate 실행

**config 파일을 작성한 후** 아래 명령어를 통해  **`Logrotate`를 실행**시켜주면 된다.

```cmd
/usr/sbin/logrotate -f /etc/logrotate.conf
```

**주기적으로 `Logrotate`를 실행하기 원한다면, `crontab`을 활용**해주면 된다.

```cmd
// 매주 일요일 자정 logrotate 실행
00 00 * * 7 /usr/sbin/logrotate -f /etc/logrotate.conf
```

**아래는 실행 관련 `Logrotate` 명령어**들이다.

```cmd
// logrotate 전체 실행
/usr/sbin/logrotate -d /etc/logrotate.conf

// 특정 logrotate process 실행
/usr/sbin/logrotate -d /etc/logrotate.d/apache

// logrotate 디버그 모드
/usr/sbin/logrotate -d /etc/logrotate.conf

// 실행 과정 화면 출력
/usr/sbin/logrotate -v /etc/logrotate.conf
```