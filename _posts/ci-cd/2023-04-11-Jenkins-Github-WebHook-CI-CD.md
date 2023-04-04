---
title: "Jenkins, Github WebHook을 활용한 CI/CD 적용"
last_modified_at: 2023-04-11T11:31
categories:
  - ci-cd
tags:
  - CI
  - CD
  - Jenkins
  - GitHub WebHook
toc: true
toc_label: "목차"
toc_icon: "sort"
toc_sticky: true
---
  

`login();`


안녕하세요. Log 입니다.

이번 Post에서는 `Jenkins`, `Github WebHook`을 활용해

간단한 `CI/CD` PipeLine을 구성해보려 합니다.

<br>

해당 적용은 **복잡한 실무용이 최종본이 아닌**,

**CI/CD 초기 구조**를 **설계, 구성에 목적**을 두고 있습니다.

실제 적용에 앞서, **구상중인 전체 CI/CD 관련 구조**에 대해 먼저 살펴보겠습니다.


# CI/CD 구조

먼저, **구상한 CI/CD의 구조를 살펴보려** 합니다.

**최종 구조**와, **MVP 구조**를 **각기 설계** 하였고,

**해당 Post**에서는 **MVP 구조를 실제 AWS EC2 상에서 구현하는데 목적**을 두고 있습니다.

## 최종 구조

구상한 최종 구조는 아래와 같습니다.

![](https://user-images.githubusercontent.com/48559894/229672798-189a77cb-0132-4a4e-8988-a49bddfb6554.png)


1. 개발 후 GitHub Repository 에 특정 `Dev` Branch 에 PR Merge
2. PR Merge시 Github WebHook이 Jenkins로 발송
3. Jenkins가 구성된 PipeLine 실행, 중간 단계 실패 시 `Slack` 특정 Channel로 메시지 발송
   1. [CI] `SonarQube Plugin`으로 코드 정적 분석 확인
   2. [CI] `Jacoco Plugin`으로 테스트 코드 Coverage 분석 확인
   3. [CI] CD 단계에서 배포할 배포 파일 생성
   4. [CD] Blue - Green 중 미구동 Port로 신규 배포파일 배포
   5. [CD] Nginx Proxy의 대상 local Port를 변경
   6. [CD] Blue - Green 중 기존 구동 Process 종료


대략적인 CI/CD **전체 흐름을 위와 같이 설계**하였습니다.

각 단계는 세부적으로 구현 시 세부적으로 증가될 수 있으나,

**최종 구조에서 적용하려는 내용**은 아래와 같습니다.


- 배포 전 정적 분석 수행
- 배포 전 테스트 Coverage 검증 수행
- 무중단 배포
- 배포 과정에 실패 시 Slack 전송


## MVP 구조


![](https://user-images.githubusercontent.com/48559894/229673116-c57be1b0-09f3-470e-8fc2-045b51142ef3.png)



최종 구조에서 `Jenkins Plugins`를 활용한 단계,

`Nginx`를 Reverse Proxy Server 로 활용한 무중단 배포 구조,

상기 **두 부분을 제거하여 가장 단순한 형태의 CI/CD 구조를 설계**하였습니다.


**단순한 구조를 먼저 구현**하고, **최종 구조까지 살을 붙여가는 방식으로 진행하기 위함**입니다.

**MVP 구조 적용을 위해선 아래와 같은 단계로 작업이 필요**합니다. 


`GitHub WebHook 설정`, `Jenkins 서버 구동`, `Jenkins PipeLine 구성`

MVP 구조 적용을 위해 각 단계별 작업을 세부적으로 살펴보겠습니다.

---

# GitHub WebHook 설정

![](https://user-images.githubusercontent.com/48559894/229678210-6cafb198-7b3f-44c0-90ca-640bcf6d0002.png)


**해당 단계**에서는 **대상 GitHub Repository**에 대해,

**WebHook Event를 발행**시킬 수 있도록,

**GitHub Webhook을 등록하는 단계**입니다.

<br>

**해당 Webhook**을 통해서, **Github Branch에서 특정 동작** 시,

**Jenkins 서버로 자동화 관련 Pipeline 실행 요청이 전송**됩니다.


해당 단계를 천천히 살펴보겠습니다.


## Webhook 등록

GitHub Repository에서 `Settings`로 이동합니다.

그 후 좌측 `Webhooks` 탭 선택 후 `Add webhook` 버튼을 클릭합니다.

![](https://user-images.githubusercontent.com/48559894/229691455-6adcd704-a391-493e-8337-9ea58ed6bdbf.png)

그 후 하기처럼, `Payload URL`, `Let me select individual events` 부분을 작성해줍니다.


![](https://user-images.githubusercontent.com/48559894/229694167-ffad55b3-3d98-46f3-865c-ea71a5d13580.png)

Payload URL 부분엔 하기와 같은 양식으로 작성해야합니다.

_/github-webhook/은 고정_


```
https://Jenkins서버주소:port/github-webhook/
```

_하기 과정에서 설치할 EC2 배포 도메인 주소 + 8080(Jenkins 기본 포트)로 작성해줍니다._


![](https://user-images.githubusercontent.com/48559894/229694968-33a7d6fd-8552-437b-8cdf-1ffae4c2e037.png)


해당 `Let me select individual events` 선택 후 나오는 목록에서

`Pull Requests`를 선택 해주세요.


마지막으로 `Active`를 체크 후 `Add webhook`을 선택해줍니다.


![](https://user-images.githubusercontent.com/48559894/229695198-cdcea00c-62f6-4ea9-a469-7969f3c0cc3a.png)


다음과 같이 등록되었다면 성공입니다.

![](https://user-images.githubusercontent.com/48559894/229702611-88b1c0a1-0efb-4cf9-a613-02d782ad2d23.png)

# Jenkins 서버 구동

이제 배포되고있는 **Linux EC2에 Jenkins를 설치하고 구동하는 단계**입니다.


## Jenkins 설치

해당 방법은 **Debian 계열의 Ubuntu OS**로

**`apt`를 사용해 설치하는 과정**을 다룹니다.

<br>




먼저 Java가 설치되어 있지 않다면 설치해줍니다.

```bash
$ sudo apt update
$ sudo apt install openjdk-11-jdk
```

<br>


`Jenkins`의 공식 저장소를 등록합니다.




```bash
$ curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  

$ echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```



apt package 목록을 업데이트합니다.

```bash
$ sudo apt-get update
```

그 후 `jenkins`를 설치합니다.

```bash
$ sudo apt-get install jenkins
```

`jenkins` service를 시작합니다.


```bash
$ sudo service jenkins start
```


우선 여기 까지 성공했다면 EC2에 Jenkins가 구동중입니다.

8080 기본 포트에 대해 구동 중인 Jenkins를 확인해보려면 아래 `lsof`를 사용합니다.

```bash
$ lsof -i:8080
```

![](https://user-images.githubusercontent.com/48559894/229942012-7b5838b1-81f4-463d-8a8e-eefbf0aac2bc.png)

<br>


---

## EC2 사양이 낮을 경우

**Jenkins를 구동할 서버의 Spec이 굉장히 낮은 상황**(AWS Free Tier 등)이라면,

`EC2 Swap 설정`, `Jenkins Executors 조절` 등의 **메모리 사용 관련 추가적인 설정이 필요할 수 있습니다**.

**해당 글을 작성한 Server의 사양은 AWS EC2 t2.micro**로,

**Memory 1GB의 굉장히 제한된 사양**이었고,

**Jenkins 구동** 이후 **Pipeline 실행 시 EC2 Instance 자체가 중단되는 Trouble이 발생**했습니다.

![](https://user-images.githubusercontent.com/48559894/231075565-525c60f3-9661-4084-88d7-72314a7427d4.png)


**서버 사양이 제한적인 경우 아래 추가 설정 진행을 권장** 드립니다.



### Swap 설정



현재 작성 글은 [[repost.aws]EC2 메모리 swap file 설정 방법](https://repost.aws/ko/knowledge-center/ec2-memory-swap-file) 을 참고하여 다음과 같은 swap 설정을 진행하였습니다.



```bash

# 128 * 8 로 1 GB swapfile 생성
$ sudo dd if=/dev/zero of=/swapfile bs=128M count=8

# swapfile 권한 변경
$ sudo chmod 600 /swapfile

# Linux 스왑 영역 설정
$ sudo mkswap /swapfile

# 스왑 공간에 스왑 파일을 추가
$ sudo swapon /swapfile

# 스왑 프로시저 등록 확인
$ sudo swapon -s

# 부팅 시 스왑 파일을 시작
# 마지막 줄에 아래 문구 입력
# LABEL=SWAP      /swapfile swap swap defaults 0 0
$ sudo vi /etc/fstab
```

단, Swap 설정으로 문제가 해결된 것은 아니며,

향후 작성할 Pipeline Script도 경량화하여 메모리를 최소화할 수 있게 끔 작성하는 등,

추가적인 고려사항들이 존재합니다.



### Jenkins Executors 조절

Jenkins의 Executors(동시 실행 가능 작업 수) 값은 기본적으로 2로 설정되어 있습니다.

이는 **저사양 EC2에서 동시작업으로 인한 Instance Down을 유발** 할 수 있어,

**사양이 낮은 EC2에서는 1로 제한하는 걸 추천**합니다.

하기 이미지 처럼, Jenkins Dashboard System 설정 창으로 이동 후,

`# of executors` 부분을 1로 변경합니다.

![](https://user-images.githubusercontent.com/48559894/231078439-d5dc2f7c-5a63-4737-8a9d-382c15874eaa.png)


### gradle 사용 시 메모리 사용량 제한 option 추가 권장 

혹여나 Pipeline Script 중 Build 진행 시, **gradle을 사용한 단계가 포함**될 경우,

하기와 같이 **메모리 사용을 제한**하고, **gradle cache를 사용하는 옵션**을 **사용하는걸 권장** 드립니다. 

```
# Before
$ ./gradlew build

# After
$ ./gradlew build -g /tmp/gradle_cache -Dorg.gradle.jvmargs=-Xmx128m
```

gradle의 기본 build jvm 메모리 사용량은 1GB로,

`-Dorg.gradle.jvmargs=-Xmx128m` 옵션으로 JVM 메모리 사용량을 제한하고,

`-g /tmp/gradle_cache` 옵션으로 캐시 디렉토리로 이전 빌드 생성 캐시를 재사용할 수 있게 끔,

관련 옵션 추가를 권장드립니다.


<br>





여기까지 저사양 EC2에서 가능한 추가 설정 관련 내용이었습니다.

---


## Jenkins Dashboard 접속

**Jenkins Dashboard 접속을 위해 방화벽 설정이 필요**합니다.

**AWS EC2에 보안 InBound 규칙에 8080 포트에 대한 TCP를 `0.0.0.0/0`**으로,

**모든 외부 요청에 대해 허용**해줍니다.

![](https://user-images.githubusercontent.com/48559894/229942219-296da50d-2e5f-4096-b323-b58af1db3be5.png)

<br>

그후 **EC2 서버 자체에서 방화벽 설정을 하기 명령어로 8080 Port에 대해 허용**해줍니다.


```bash
$ sudo firewall-cmd --permanent --add-port=8080/tcp
$ sudo firewall-cmd --reload
```

<br>

**여기 까지 설정**했다면, **EC2 주소 + Jenkins Port 주소**로,

**Jenkins Dashboard를 접속**할 수 있습니다.


![](https://user-images.githubusercontent.com/48559894/229942662-b99f881d-9517-4a85-b119-e3cce2e28b81.png)

**Jenkins Dashboard 로그인을 위해 하기 명령어**로,

**jenkins 초기 비밀번호를 확인**합니다.

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

그 후 Dashboard에 입력해줍니다.

<br>

로그인에 성공하면 아래와 같은 창으로 이동됩니다.

![](https://user-images.githubusercontent.com/48559894/229943105-eec8edec-7a63-40dc-a337-1df48d4f4ba9.png)

`Select plugins to install`을 눌러,

추후 plugins는 추가할 수 있으니,

MVP 단계에서는 하기 Plugins만 설치하고 넘어갑니다.

- Git
- GitHub
- Pipeline
- GitHub Integration

![](https://user-images.githubusercontent.com/48559894/229943764-192bda83-5dd3-4321-9b33-262f49ed6328.png)

![](https://user-images.githubusercontent.com/48559894/231057449-bbabb6cb-5dab-4907-89e6-b567e12a427a.png)

install 진행 후에는 아래처럼 초기 관리자 등록 화면이 나옵니다.

![](https://user-images.githubusercontent.com/48559894/229946469-52fcfdf7-befa-4bb1-9f28-d4d2870e45d4.png)


알맞게 입력 후 `Save and Continue`로 다음단계로 넘어갑니다.


![](https://user-images.githubusercontent.com/48559894/229946774-1b035fd8-4c38-454b-8845-da0c647e9d18.png)

마지막으로 Jenkins URL을 지정해주고 `Save and Finish` 로 설정을 마칩니다.


![](https://user-images.githubusercontent.com/48559894/229970974-8ea2cfb2-ab91-43c8-a82d-cc66de196714.png)

이제 `EC2도메인:JenkinsPort`로 `Jenkins Dashboard`에 접속할 수 있습니다.


## Github Global Credential 등록

해당 단계에서는 Jenkins에서 Git, GitHub 관련 Plugin에서 

Global하게 사용할 인증값을 등록하겠습니다.


### GitHub AccessToken 생성


먼저 GitHub AccessToken을 발행해줍니다.

![](https://user-images.githubusercontent.com/48559894/231058897-49be86a9-c27a-4914-bbdc-0bcc8270eefc.png)

GitHub로 이동 후 Profile 이미지 선택 > `Settings`로 이동한 후,

`Developer Settings` > `Personal access tokens`로 이동해

`Tokens(class)` > `New personal access token (classic)` 을 클릭 해 생성 화면으로 이동 합니다.



![](https://user-images.githubusercontent.com/48559894/231059447-de3d6aad-e5dc-4990-9dbe-09ef40d23e2c.png)


이후 해당 AccessToken의 권한으로 상기 이미지와 같이 `Repo`, `repo_hook` 권한을 선택 후 생성해줍니다.


### Jenkins Global Credential 등록

이제 위에서 등록한 Github AccessToken을 Jenkins에 전역 Credential로 등록 해줍니다.

![](https://user-images.githubusercontent.com/48559894/231058352-8da06695-c0db-4a6a-954d-7e9fc3c82c7a.png)


상기 경로로 이동 후 `Add Credentials`를 클릭해 줍니다.

![](https://user-images.githubusercontent.com/48559894/231059953-16508a69-46a2-44f0-9683-251b56dba1cc.png)

`Credential` 생성 화면에서 

`Kind`는 Secret text,

`Scope`는 Global,

`Secret`은 상기 과정에서 등록한 Github AccessToken,

`ID`는 식별 가능하도록 아무 값이나 입력해주시면 됩니다.

그 후 `Create`으로 생성해줍니다.

### GitHub Server 등록


이제 **마지막으로 등록한 Jenkins Credential이, GitHub Server에서 인증값으로 사용될 수 있도록**,

**GitHub Server를 등록하며 Credential을 사용하도록 설정**하겠습니다.

![](https://user-images.githubusercontent.com/48559894/231060360-bf84a6e7-d7ee-4287-8eb6-a89fc8eda4f7.png)

상기 이미지 처럼 `Dashboard` > `Jenkins 관리` > `System` 으로 이동합니다.


![](https://user-images.githubusercontent.com/48559894/231060763-7319632a-e432-4918-bd1c-d6efbadd5627.png)

`GitHub` 탭에서 화면과 같이 `https://api.github.com` 을 API URL에 입력 후,

상기 과정에서 등록한 `Credential`을 지정해 줍니다.

그리고 `Manage hooks`를 선택해 줍니다.


<br>

이로써 **Jenkins에서 GitHub Plugin을 위한 설정 과정이 끝**났습니다.


---

# Jenkins Pipeline 구성

이제 간단한 Pipeline을 구성해보겠습니다.

![](https://user-images.githubusercontent.com/48559894/229972296-63877e48-5c3f-4c2b-b104-2845743ee7b9.png)


## Pipeline 흐름

구현하려는 Pipeline의 흐름은 다음과 같습니다.

- [CI 단계]
  - GitHub Repository, 특정 Branch(dev) Pull Request Merged 됨
  - 특정 Branch(dev)기준 소스 코드를 EC2에 받아옴

- [CD 단계]
  - CI 단계에서 가져온 소스코드를 Build
  - 현재 구동중인 Process를 종료함
  - 신규 배포 파일로 구동함 


## PipeLine 생성

Jenkins Dashboard로 앞서 살펴본 Pipeline을 생성하겠습니다.

`CI`, `CD` 두 가지로 나누어 각각 생성합니다 

### CI PipeLine

![](https://user-images.githubusercontent.com/48559894/229972877-84db8e8d-e308-4130-8d72-c50da2a31af5.png)


Dashboard에서 `새로운 Item` 부분을 클릭합니다.


![](https://user-images.githubusercontent.com/48559894/229973012-cf6733a6-8885-4475-8fed-624403f56563.png)

다음 화면에서 `Job Name` 지정 후, `Pipeline` 선택 후 `OK`로 Project를 생성합니다.


이제 세 단계로 나누어 해당 Pipeline Project를 작성하겠습니다.


`GitHub Project 지정`, `Build Triggers 설정`, `Pipeline 작성`


#### GitHub Project 지정

먼저 해당 Pipeline이 동작하는 GitHub Project의 Repository 주소를 입력해줍니다.

![](https://user-images.githubusercontent.com/48559894/231063949-c223c428-bf70-4d0f-8ebd-9fa687b0341e.png)


#### Build Triggers 설정

하기 이미지 처럼, `GitHub Branches`를 클릭 후,

`Trigger Mode`는 `Hooks with Presisted Data`를 선택하고,

`Trigger Events` 부분에 `Add`를 통해,

`Hash Changed`를 추가해줍니다.

![](https://user-images.githubusercontent.com/48559894/231064234-e9a14774-3e06-486a-b780-a93b31d6aa02.png)

이를 통해, `Github Branch`에 Hash값이 변경되는 Hook Event가 발생 시 해당 Build가 동작되게 됩니다.


#### Pipeline 작성

pipeline 탭에서 하기와 같이 간단한 Groovy Script를 작성해줍니다.

```groovy
pipeline {
   agent any
   stages{
       stage('SCM') {
           steps {
               git branch: "dev",
               url: "https://github.com/???????.git"
           }
       }
   }
}
```

#### 테스트

여기 까지 **작성된 Pipeline을 한번 정상 동작하는지 확인**해보겠습니다. 


먼저 **대상 Repository**에 대해,

PR 등의 작업을 통해 **Hook Event를 발생**시켜 봅니다.

발생한 WebHook Event 는 해당 Repository의 Settings의 Webhooks 탭에서 확인할 수 있습니다.

![](https://user-images.githubusercontent.com/48559894/231066211-1c59dc6f-cfb4-44df-bf5f-b2a2cea3d386.png)

![](https://user-images.githubusercontent.com/48559894/231066310-4b82b4e0-fe1c-4c3a-af2f-936bb9e95c41.png)


상기 이미지 처럼 **발송되는 Github 측에서의 Webhook을 확인 가능**하고,

`Redeliver` 버튼 클릭을 통해 재전송 해볼 수 있습니다.


![](https://user-images.githubusercontent.com/48559894/231066961-9c0ba2c9-7b6a-49ed-b0b0-99854b7a5c10.png)


작성한 Pipeline Project에 Build가 생성되어 `#??` 형태로 작성되었다면,

GitHub Webhook에 의해 동작하여, CI Build 단계 까지 성공입니다.

<br>


혹시나 작성한 Pipeline Project에 Build가 생성되지 않는다면,

하기 이미지와 같이 이동 후 Jenkins 서버 Log를 살펴봅니다.

![](https://user-images.githubusercontent.com/48559894/231067249-0b6f6a25-3e4f-4b18-bac4-36b1d3f4b5fd.png)

<br>

**정상적으로 Build 동작** 시,

EC2 상에 `/var/lib/jenkins/workspace/Jenkins-PipeLine-프로젝트명` 경로로 이동 시,

해당 **Repository Source 본이 생성**되어 있습니다.

<br>

이를 통해 **Dev, Main 등의 Branch에 Hash 값이 변경**되는,

**Hook Event(PR Merged)가 발생** 시,

**Jenkins 서버로 해당 소스본을 통합**하여 가져오는, **CI 단계에 대한 간단한 구축이 완료**되었습니다.

### CD PipeLine

![](https://user-images.githubusercontent.com/48559894/229972877-84db8e8d-e308-4130-8d72-c50da2a31af5.png)

Dashboard에서 `새로운 Item` 부분을 클릭합니다.

![](https://user-images.githubusercontent.com/48559894/229973012-cf6733a6-8885-4475-8fed-624403f56563.png)

다음 화면에서 `Job Name` 지정 후, `Pipeline` 선택 후 `OK`로 Project를 생성합니다.

CD Pipeline Project는 하기 두 과정을 거쳐 작성해보겠습니다.


`Build Triggers 설정`, `Pipeline 작성`


#### Build Triggers 설정


![](https://user-images.githubusercontent.com/48559894/231071239-8bc87766-21ce-45a8-be9b-657fb4744fd7.png)

상기 이미지 처럼 `Build Triggers` Tab으로 이동 후,

`Build after other projects are built`를 선택하고,

이전에 작성한 CI Pipeline Project 명을 입력해주시면,

자동 완성 검색 창에 작성한 Project를 확인할 수 있습니다.

선택한 후 `Trigger only if build is stable`을 지정해 줍니다.


**해당 과정을 통해, CD Pipeline Project**는

**CI Pipeline Project가 안정적으로 Build된 이후에 동작되도록 설정**하였습니다.


#### Pipeline 작성

CD 단계에서는 하기 4과정을 거치도록 구성되었습니다.

- 배포 파일 Build, `Build`

- 배포 파일 Application 배포 경로로 복사, `Copy Jar`

- 기존 Application 중단, `Stop current process`

- 신규 Application 구동, `Start Application`



간단한 Groovy Script는 다음과 같습니다.

```groovy
pipeline {
  agent any

  environment {
    PROJECT_PATH = "/var/lib/jenkins/workspace/?????"
    BATCH_PATH = "/원하는/폴더명"
    GRADLE_OPTS = "-Xmx128m"
    JAVA_OPTS = "-Xmx128m"
  }

  stages {
    stage('Build') {
      steps {
        sh "cd ${PROJECT_PATH} && ./gradlew build -g /tmp/gradle_cache -Dorg.gradle.jvmargs=\"${GRADLE_OPTS}\""
      }
    }

    stage('Copy jar') {
      steps {
        sh "cp ${PROJECT_PATH}/build/libs/*.jar ${BATCH_PATH}/"
      }
    }

    stage('Stop current process') {
      steps {
        sh "while [[ \$(lsof -i:80) ]]; do lsof -i :80 | awk '{print \$2}' | xargs kill -9 ; sleep 1; done"
        sh "echo 'There is no process using 80 port.'"
      }
    }

    stage('Start Application') {
      steps {
        sh "nohup java ${JAVA_OPTS} -jar ${BATCH_PATH}/*.jar > /dev/null 2>&1 &"
      }
    }
  }
}
```

작성된 CD Pipeline Project는 

하기와 같이 CI 단계의 Project의 build로 부터 CD Project가 Build되는 걸 확인 할 수 있습니다.

![](https://user-images.githubusercontent.com/48559894/231072819-2bf0e749-61d5-49b6-be7b-67d01bcf8b15.png)


<br>


---


여기까지 `Jenkins` + `GitHub Webhook`을 활용한 간단한 CI / CD 구축 과정을 살펴봤습니다.


감사합니다.


`logout();`
