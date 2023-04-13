---
title: "Nginx, Jenkins를 활용한 CI/CD 무중단 배포"
last_modified_at: 2023-04-17T08:39
categories:
  - ci-cd
tags:
  - CI
  - CD
  - Jenkins
  - Nginx
  - 무중단 배포
toc: true
toc_label: "목차"
toc_icon: "sort"
toc_sticky: true
---
  

`login();`

안녕하세요. Log입니다.

오늘은 지난 시리즈들에 이어, Nginx를 활용해 무중단 배포 구조를 적용해보려 합니다.

지금까지 적용된 구조는 다음과 같습니다.

![](https://user-images.githubusercontent.com/48559894/231669123-ecc6bdeb-e461-4d33-8661-0dbfaa252357.png)

[이전 구축 과정을 살펴보려면 여기로](https://gil-log.github.io/ci-cd/Jenkins-Github-WebHook-CI-CD/)


현재 간략하게 구축된 구조는 아래와 같습니다.

[CI 단계]
- GitHub Repository, 특정 Branch(dev) Pull Request Merged 됨
- 특정 Branch(dev)기준 소스 코드를 EC2에 받아옴

[CD 단계]
- CI 단계에서 가져온 소스코드를 Build
- 현재 구동중인 Process를 종료함
- 신규 배포 파일로 구동함

이번 포스팅에서는 `[CD 단계]`에 Nginx Proxy Server를 앞단에 두어,

중간에 구동중인 Process가 종료되고, 신규 배포 파일로 구동되는 사이의 중단 텀을 극소화하는,

무중단 배포 구조를 적용해보려 합니다.


# 무중단 배포 설계 구조

이번 포스팅에서 적용하려는 부분은 아래와 같습니다.

![](https://user-images.githubusercontent.com/48559894/231670700-e296e4b5-d2c0-4e52-a44f-69cf1af4c674.png)

기존 `CD` 단계를 **Nginx 서버를 Proxy Server로 앞단**에 두어,

**Blue Port, Green Port 신 버전이 들어 올때 마다, 두 포트를 번갈아 구동**하며

**신규 버전이 구동되는 동안**에는 **기존 Port로 접속을 유지해 서비스가 중된되지 않고**,

**신규 버전 구동이 완료된 후**에는 **신규 버전 Port를 Nginx에서 바라보도록 변경**한 후,

**기존 Port를 죽여 중단되지 않는 무중단 배포를 적용**하려 합니다.

그림으로 살펴보면 아래와 같습니다.

![](https://user-images.githubusercontent.com/48559894/231672992-0ea71a7b-0c3f-40d1-9aee-cc8fa4285b0b.png)

위와 같은 방식으로 적용하기 위해 아래 단계 별로 진행해보겠습니다.

# Nginx

먼저 **Nginx 관련 설치 및 설정이 필요**합니다.

**Nginx를 설치**하고, **Nginx가 Proxy Server로 구동** 및,

**Jenkins의 CD 단계 때 동적으로 Instance 구동 Port를 변경할 수 있도록 설정 파일을 생성**해보겠습니다. 

## Nginx 설치

먼저 Nginx를 설치합니다.

```shell
# apt 목록 업데이트
$ sudo apt update

# nginx 설치
$ sudo apt install nginx

# nginx 실행
$ sudo systemctl start nginx

# HTTP, HTTPS 요청 허용
$ sudo ufw allow 'Nginx Full'
```

`lsof -i:80` 으로 기본 80포트로 Nginx가 구동되어 있는지 확인해주세요.

![](https://user-images.githubusercontent.com/48559894/231680159-e2d8e3d9-411a-47da-8911-acbf75122de3.png)

여기까지 Nginx 정상 구동을 확인 하셨다면 이제, Nginx를 ProxyServer로 설정해보겠습니다.

## Nginx Proxy Server 설정

`cd /etc/nginx/` 로 Nginx가 설치된 경로로 이동합니다.

해당 경로에서 작업할 파일과 작업할 내용은 아래와 같습니다.

- nginx.conf
  - Nginx 메인 설정 파일로, 기본 sites-enabled값 주석처리 작업
- conf.d
  - nginx.conf에서 `conf.d/*.conf` 파일들이 기본값으로 Include 되어 있어, <br> `/`에 대해 ProxyServer 설정 및 `Upstream` 설정을 각 파일로 나누어, 동적으로 수정될 수 있게끔 신규 파일 생성


먼저 `nginx.conf` 파일에 대해 작업해보겠습니다.

### nginx.conf 파일 수정

`/etc/nginx/nginx.conf` 파일을 하기 `vi` 명령어로 수정합니다.

```shell
$ vi /etc/nginx/nginx.conf
```

하단으로 내려오면 아래와 같은 구문이 존재할텐데

```
        ,,,
        
        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}

```

여기서 **아래 이미지와 같이 `include /etc/nginx/sites-enabled/*;` 부분을 주석 처리**해,

**기본 Nginx의 Server 설정 Block을 Disable 처리** 하겠습니다.

![](https://user-images.githubusercontent.com/48559894/231682017-d690c000-a5ff-4345-b6ec-7621245cb9f1.png)


여기까지 수행하면 현재 Nginx에서는 Server에 대한 기본 접속 설정이 존재하지 않는 상태가 됩니다.

이제 다음 `/etc/nginx/conf.d` 경로로 가 신규 설정파일 들을 생성해보겠습니다.

### /etc/nginx/conf.d에 신규 conf 추가

앞서 `nginx.conf`에서 살펴보았듯이, `include /etc/nginx/conf.d/*.conf;` 블록으로 인해,

**`/conf.d/` 폴더 밑에 `.conf`로 끝나는 파일들은 Nginx 설정에 포함**되게 됩니다.

이곳에 각각 `backend.conf` 와 `server.conf` 두가지 `.conf` 파일을 생성하겠습니다.

#### backend.conf

`/etc/nginx/conf.d` 경로에 `backend.conf`라는 파일을 생성해줍니다.

_파일명은 상관없습니다._

```shell
$ vi /etc/nginx/conf.d/backend.conf
```

그리고 하기 내용을 입력해줍니다.

```
upstream backend {
    server 127.0.0.1:8081;
}
```

해당 내용은 `backend`라는 upstream block에 `localhost:8081`을 등록해준 작업입니다.

최초 구동될 Port로 `8081`을 사용하고 추후 CD 스크립트 작성 단계 때는,

해당 `backend.conf` 파일의 port 부분을 수정할 예정입니다.

작성을 마쳤다면, 이제 `server.conf`를 생성해줍니다.

#### server.conf

`/etc/nginx/conf.d` 경로에 `server.conf`라는 파일을 생성해줍니다.

_파일명은 상관없습니다._

```shell
$ vi /etc/nginx/conf.d/server.conf
```

그리고 하기 내용을 입력해줍니다.

```
server {
    listen 80;

    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

```

Nginx의 기본 80포트에 대해 `/` 경로에 대한 proxy 설정 관련 작성입니다.

여기서 `proxy_pass`로 `http://이전에작성한backend라는upstream블록명`을 입력하여,

`backend.conf`에서 작성한 `Upstream`을 사용할 수 있도록 되어있습니다.

여기까지 작성후 하기 명령어로 Nginx를 reload 해줍니다.

```shell
$ sudo service nginx reload
```


여기까지 Nginx 설치 및 설정을 마쳤습니다.

이제 해당 서버의 80포트에는 Nginx가 구동되고, 요청에 대해
8081 Port에 구동중인 Process로 전달하게 됩니다.

이제 CD 단계 Jenkins Script를 수정하겠습니다.

# Jenkins 

## jenkins sudo 권한 부여

**변경하려는 CD 단계**에서 **구동중인 Nginx에 대해 설정 파일 reload 등의 권한**을,

**root 권한으로 구동해야하는 부분이 존재**합니다.

따라서 `jenkins` 사용자에 대해 **아래와 같은 과정으로 `nginx -s reload` 명령어를 sudo로 구동 가능하게 권한을 부여**합니다.

```shell
# sudo 그룹에 Jenkins 사용자를 추가합니다.
$ sudo usermod -aG sudo jenkins

# /usr/sbin/nginx 파일에 대한 실행 권한을 부여합니다.
$ sudo chmod +x /usr/sbin/nginx

# sudoers 파일을 열어줍니다.
$ sudo visudo

```

`visudo`로 jenkins 사용자에게 nginx -s reload 명령어를 실행할 수 있는 권한을 부여합니다.

아래 문구를 파일의 마지막에 추가합니다.

```
jenkins ALL=NOPASSWD: /usr/sbin/nginx -s reload
```

![](https://user-images.githubusercontent.com/48559894/232634813-1539540e-59ae-42f0-9ea3-27504d157dfe.png)


## Pipeline Script 수정

현재 작성해야하는 Jenkins의 무중단 배포 단계는 아래와 같습니다.

- 인스턴스가 구동중인 Port를 찾는다.
- 해당 Port 반대 Port로 신규 버전을 구동한다.
- 구동이 완료되면 Nginx의 Upstream Port를 변경한다.
- Nginx설정을 Reload한다.
- 기존 인스턴스가 구동중이던 Port의 Process를 종료한다.

해당 무중단 배포가 포함된 Pipeline Script는 아래와 같습니다.

```groovy

pipeline {
  agent any

  environment {
    PROJECT_PATH = "/var/lib/jenkins/workspace/CI"
    BATCH_PATH = "/app/api"
    GRADLE_OPTS = "-Xmx128m"
    JAVA_OPTS = "-Xmx128m"
  }

  stages {
      
    stage('CD Run') {
        steps {
            sh "echo '[CD] Run'"
        }
    } 
     
    stage('Build') {
      steps {
        sh "cd ${PROJECT_PATH} && ./gradlew bootJar -g /tmp/gradle_cache -Dorg.gradle.jvmargs=\"${GRADLE_OPTS}\""
      }
    }

    stage('Copy jar') {
      steps {
        sh "cp ${PROJECT_PATH}/build/libs/*SNAPSHOT.jar ${BATCH_PATH}/"
      }
    }

    // 인스턴스가 구동중인 Port를 찾는다.
    stage('Find current port') {
        steps {
            script {
                currentPort = sh(
                    script: "lsof -i :8081 -sTCP:LISTEN -t || echo '8082'",
                    returnStdout: true
                ).trim()
                
                currentPort = currentPort == '8082' ? '8082' : '8081'
            }
            echo "Current port: ${currentPort}"
        }
    }
    
    // 해당 Port 반대 Port로 신규 버전을 구동한다.
    // 구동 까지 대기
    stage('Start new version') {
        steps {
            script {
                newPort = currentPort == '8082' ? '8081' : '8082'
    
                sh "JENKINS_NODE_COOKIE=dontKillMe nohup java ${JAVA_OPTS} -jar -Dserver.port=${newPort} ${BATCH_PATH}/*SNAPSHOT.jar --spring.profiles.active=dev > /dev/null 2>&1 &"
    
                timeout(time: 1, unit: 'MINUTES') {
                    waitUntil {
                        sh(
                            script: "lsof -i :${newPort} -sTCP:LISTEN -t",
                            returnStatus: true
                        ) == 0
                    }
                }
    
                echo "Started new version on port ${newPort}"
            }
        }
    }
    
    // 구동이 완료되면 Nginx의 Upstream Port를 변경한다.
    stage('Change upstream port') {
        steps {
            script {
                def upstreamFile = "/etc/nginx/conf.d/backend.conf"
                def currentUpstream = sh(
                    script: "grep -oP '(?<=server ).*(?=;)' ${upstreamFile}",
                    returnStdout: true
                ).trim()
    
                def newUpstream = currentUpstream.replace(currentPort, newPort)
    
                sh "sed -i 's/${currentUpstream}/${newUpstream}/g' ${upstreamFile}"
    
                echo "Changed upstream port to ${newPort}"
            }
        }
    }
    
    //  Nginx 설정을 Reload 한다.
    stage('Reload Nginx') {
        steps {
            sh "sudo nginx -s reload"
            echo "Reloaded Nginx"
        }
    }
    
    // 기존 인스턴스가 구동중이던 Port의 Process를 종료한다.
    stage('Stop old version') {
        steps {
            sh """
                #!/bin/bash
                while [[ \$(fuser -n tcp -k ${currentPort}) ]];
                do sleep 1; 
                done
            """
            sh "echo 'There is no process using ${currentPort} port.'"
        }
    }
  }
}

```


여기까지 Nginx를 활용해, Jenkins CD 단계를 무중단 배포로 적용하는 과정을 살펴보았습니다.

감사합니다.

`logout();`
