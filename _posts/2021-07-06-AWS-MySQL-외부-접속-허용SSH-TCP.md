---
title: "[AWS] MySQL 외부 접속 허용(SSH, TCP)"
last_modified_at: 2021-07-06T23:23
categories: 
  - aws
tags: 
  - 'aws' 
  - 'mysql' 
  - 'ssh' 
  - 'tcp'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[[Mysql] 외부 접속 허용[NOT-NULL 디벨로펀님]](https://not-null.tistory.com/16)


# MySQL 접속 계정 외부 접속 허용

**AWS Linux Server에 접속해 `mysql -r -p`로 `root` 계정 접속 후**,

**외부 접속을 희망하는 계정에 외부 접속을 허용**해준다.

```cmd
// root 계정 접속
mysql -r -p

// 해당 user명에게 모든 DB 스키마에 대한 권한을 %(어디서든 외부 접속, 내부 접속) 허용해준다
grant all privileges on *.* to 'user명'@'%';

// 권한 설정 내용 flush 적용
flush privilegs;
```

![](https://images.velog.io/images/gillog/post/ae42b7d0-eeb0-4be5-a0d7-17483549f160/image.png)

# SSH 접속

**AWS Instance 접속에 발급한 `.pem` Key File을 이용**해 **SSH 포트로 접속 후 > 내부 망 IP 주소의 3306 Port로 접속하는 방법**이다.

**해당 방법**은 **별다른 설정 없이도 AWS Instance에 성공적으로 접속 했다면(Putty 등) 바로 접속 가능한 방법**이다.
_단 **Tomcat 등의 WAS 에서 접속 시에는 `jsk`, `pkc` 포맷 KeyStore 인증서를 등록해야 하니 추가 작업 필요**_


![](https://images.velog.io/images/gillog/post/7dc8ee88-7924-49ef-b623-9560277ea24f/image.png)

# TCP 접속

**해당 방법**은 **Tomcat이나 다른 WAS를 사용하는 어디서든 다른 인증서 발급 없이 접속 가능한 가장 범용적인 방법**이다.


## 1. AWS 인바운드 규칙 설정

먼저 **AWS MySQL이 설치된 Instance**에 **보안그룹의 인바운드 규칙**에 **`0.0.0.0/0` `TCP` `3306` Port를 열어준다.**
_0.0.0.0/0 은 모든 IP주소에서 접속을 허용해 준다. 굉장히 위험하니 주의하자_
_BitCoin 채굴러들이나 심심한 보안쪽 해커들의 장난감이 될 수 있다._
_특정 IP마다 할당해주는 것이 Best다. [EX]12.415.12.14/0_

![](https://images.velog.io/images/gillog/post/dbfbd4ba-9be0-49d7-9424-de560c5faebe/image.png)

## 2. MySQL conf 외부 접속 허용 설정

**AWS Linux 서버에 있는 `/etc/mysql/mysql.conf.d` 경로의 `mysqld.cnf` 파일안에 `bind-address`를 `0.0.0.0` 으로 변경**해주고,

**MySQL을 재시작** 해주어야 한다.

```cmd
// mysql.conf.d 경로로 이동
cd /etc/mysql/mysql.conf.d/

// mysqld.cnf 파일 수정
// i 입력으로 수정모드 전환 후 bind-adress를 0.0.0.0으로 변경
// :wq 나 :!wq로 작성 후 작성 종료
vi mysqld.cnf

// mysql 재시작
service mysql restart
```

![](https://images.velog.io/images/gillog/post/bbd713d4-2e69-484e-9fa8-4c033d24355e/image.png)

![](https://images.velog.io/images/gillog/post/ced527bd-078a-4e58-9527-6524f8235be6/image.png)

![](https://images.velog.io/images/gillog/post/b63c3401-f14a-4843-a352-c147212ca896/image.png)

## 3. MySQL WorkBench 접속

**AWS Instance의 퍼블릭 IPv4 주소나 도메인 주소와 MySQL Port 번호**,

**MySQL 접속 계정 등의 정보를 입력**하면 된다.

![](https://images.velog.io/images/gillog/post/5a5de795-6296-4c32-8cb3-25628678d1d6/image.png)