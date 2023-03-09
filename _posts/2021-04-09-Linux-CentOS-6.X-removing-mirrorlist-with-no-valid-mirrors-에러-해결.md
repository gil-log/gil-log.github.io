---
title: "[Linux] CentOS 6.X removing mirrorlist with no valid mirrors 에러 해결 "
last_modified_at: 2021-04-09T04:58
categories: 
  - error
tags: 
  - 'CentOS 6.9' 
  - 'linux' 
  - 'mirrorlist' 
  - '에러'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---


# 🙆‍♂️ import 🙇‍♂️

[CentOS 6 yum update 오류 해결 방법[Shine your light]](https://lhjin.tistory.com/entry/CentOS-6-yum-update-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95)

[]()

[]()

[]()

[]()

[]()

<br>


# 에러 상황


![](https://images.velog.io/images/gillog/post/7312591e-981b-4f4c-af61-033c19786723/image.png)

```bash
[root@DEVEL1 local]yum install ?y https://centos6.iuscommunity.org/ius-release.rpm

Loaded plugins: fastestmirror, security
Setting up Install Process
Loading mirror speeds from cached hostfile
YumRepo Error: All mirror URLs are not using ftp, http[s] or file.
 Eg. Invalid release/repo/arch combination/
removing mirrorlist with no valid mirrors: /var/cache/yum/i386/6/base/mirrorlist.txt
Error: Cannot find a valid baseurl for repo: base
```


**Centos 6.9 버전을 사용 중**인데, Python3 버전대를 사용하기위해 **yum 패키지 설치 과정 중 위와 같은 에러가 발생**했다.

```
removing mirrorlist with no valid mirrors: /var/cache/yum/i386/6/base/mirrorlist.txt
Error: Cannot find a valid baseurl for repo: base
```

**핵심 에러 문구**는 위와 같다. 

검색해보니 **CentOS 6 버전대에 대한 지원 종료**로 인해, **yum 패키지 설치에 애로사항**들이 많은 모양이다.


# 에러 해결


먼저 자신의 **Centos Release 버전을 확인**해야 한다.

```bash
cat /etc/centos-release
```
위 명령어를 사용하면 자신의 CentOS 버전을 확인할 수 있다.

![](https://images.velog.io/images/gillog/post/a675aabc-5e49-4de0-ac1d-f5c9e4876d25/image.png)

이후 **자신의 구동 Bit 환경**을 알아야 한다.

```bash
getconf LONG_BIT
```

위 명령어로 자신의 OS가 32Bit인지 64Bit인지 확인한다.

![](https://images.velog.io/images/gillog/post/6676c7e0-a170-4e0c-9570-dd501c6e2051/image.png)


이후 **CentOS Bits에 맞게 아래 명령어를 순서대로 입력**한다.

아래 명령어를 입력하기 전 자신의 CentOS 버전이 들어갈 수 있도록 수정해주어야한다.
`https://vault.centos.org/centos-os 버전/os/i386/` 
*필자는 6.9버전이므로 6.9를 적용*

#### 32Bit

```bash
echo "https://vault.centos.org/6.9/os/i386/" > /var/cache/yum/i386/6/base/mirrorlist.txt
echo "http://vault.centos.org/6.9/extras/i386/" > /var/cache/yum/i386/6/extras/mirrorlist.txt
echo "http://vault.centos.org/6.9/updates/i386/" > /var/cache/yum/i386/6/updates/mirrorlist.txt
```

#### 64Bit

```bash
echo "https://vault.centos.org/6.9/os/x86_64/" > /var/cache/yum/x86_64/6/base/mirrorlist.txt
echo "http://vault.centos.org/6.9/extras/x86_64/" > /var/cache/yum/x86_64/6/extras/mirrorlist.txt
echo "http://vault.centos.org/6.9/updates/x86_64/" > /var/cache/yum/x86_64/6/updates/mirrorlist.txt
```

![](https://images.velog.io/images/gillog/post/d0eebd9d-4cc2-4248-8bdd-0a87007400d6/image.png)

**이후 yum 으로 패키지 설치를 진행하면 mirrorlist 관련 에러 메시지가 없어진다.**



혹시나 

```
removing mirrorlist with no valid mirrors: /var/cache/yum/i386/6/base/mirrorlist.txt
Error: Cannot find a valid baseurl for repo: base

removing mirrorlist with no valid mirrors: /var/cache/yum/i386/6/extras/mirrorlist.txt
Error: Cannot find a valid baseurl for repo: extras

removing mirrorlist with no valid mirrors: /var/cache/yum/i386/6/updates/mirrorlist.txt
Error: Cannot find a valid baseurl for repo: updates
```


**위와 같은 메시지가 또 발생할 경우** **`for repo: ????` 저 해당 문구**에 해당하는 **명령어를 입력**해주면 된다.


```bash
echo "http://vault.centos.org/6.9/????/i386/" > /var/cache/yum/i386/6/????/mirrorlist.txt
```

물론 echo 다음의 URL 경로가 실존하는지 확인해야 한다.
