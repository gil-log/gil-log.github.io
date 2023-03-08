---
title: "[MacOS] 사용중인 Port 죽이기, Port Kill"
last_modified_at: 2021-07-06T00:11
categories: 
  - macos
tags: 
  - 'Kill' 
  - 'PORT' 
  - 'linux' 
  - 'lsof' 
  - 'mac'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# lsof -i :portnum

**`lsof -i :portnum` 명령어**로 **사용중인 해당 Port의 `PID`를 확인**할 수 있다.

```cmd
gillog@Gillogui-MacBookAir DevTool % lsof -i :80
COMMAND   PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
java    17689 gillog  156u  IPv6 0xfd06b486a616f0b3      0t0  TCP *:http (LISTEN)
```

# kill -9 portnum

**`kil -9 portnum` 명령어**로 **해당 `PID`의 Process를 종료**할 수 있다.

```cmd
gillog@Gillogui-MacBookAir DevTool % kill -9 17689
```
![](https://images.velog.io/images/gillog/post/0e771d04-eb89-463c-9a64-120724d4c74a/image.png)