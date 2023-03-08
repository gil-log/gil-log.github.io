---
title: "[Intellij] lombok 사용하기(Gradle, Plugin, Build Annotation 설정 포함)"
last_modified_at: 2021-12-03T00:53
categories: 
  - ide
tags: 
  - 'IntelliJ' 
  - 'LomBok' 
  - 'gradle'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---


# lombok 설정

**`Intellij` IDE를 사용** 할 때 **`lombok` plugin까지 모두 설정**했지만,

**`lombok` annotation이 동작하지 않을 때**가 있다.

오늘은 **`Intellij`에서 `lombok` 사용을 위한 과정을 살펴보려 한다.**

<br>

## 요약

1. **`lombok` Dependency 추가**

2. **`lombok` plugin 설치**


3. **`Settings` >`Compiler` > `Annotation Processors` 
Enable 설정**

4. **`Compile` 에러 발생**시
**`build.gradle`에 `annotationProcessor` 설정 추가**

---

# Intellij lombok 설정


## lombok Dependency Compile

**`Gradle`로 `Dependency` 관리를 기준으로** 아래 처럼,

**`lombok` compile을 설정**하고,

![](https://images.velog.io/images/gillog/post/41a8b779-61b7-4532-b54b-abf1fc51022c/image.png)

```gradle
dependencies { 
	compile('org.projectlombok:lombok:1.18.22')
}
```

**`Gradle`을 `Build`해 `External Library`에 정상 설치를 확인**해준다.

![](https://images.velog.io/images/gillog/post/cbb61ffe-c5d8-4d02-a91a-87c345030be9/image.png)

## lombok Plugin

**`File` > `Settings` > `Plugins`로 이동**해

**`Intellij` `lombok` plugin 까지 설치** 해주자.

![](https://images.velog.io/images/gillog/post/38701de7-3680-4297-9de6-e730a16ea7c0/image.png)




## Settings Annotation Processors 설정

**다른 `IDE`에서는 `lombok.jar`를 실행**시켜,

**`IDE`에 `Install` 하는 과정이 존재**하는데,

**`Intellij`에서는 `Settings`에서 관련 설정을 마무리** 지으면된다.
_이미 Plugin은 설치했기 때문_


<br>

**`File` > `Settings` > `Build, Execution, Deployment` > `Compiler` > `Annotation Processors` 까지 이동**해주자.

![](https://images.velog.io/images/gillog/post/5831acc6-28ab-47cc-ad4f-f7734b9f75df/image.png)

여기서 **`Enable annotation processing`을 설정**해주자.

<br>

## Error 발생?

**여기 까지 설정**했다면,

**Editor 상에서 lombok 관련 import나 method 사용도 이상없다.**


![](https://images.velog.io/images/gillog/post/9c17424f-782f-462b-a39c-001ba74be560/image.png)

![](https://images.velog.io/images/gillog/post/05853aff-21f1-4a4e-95e7-e65a8343a8ff/image.png)

하지만 **실제 Compile 때는 아래와 같은 에러가 발생**할 수 있다.

![](https://images.velog.io/images/gillog/post/eae11b9c-ad55-413b-8aa1-0df0c3f6b916/image.png)

```
TestController.java:14: error: cannot find symbol
            result = testDTO.getTest();
                            ^
  symbol:   method getTest()
  location: variable testDTO of type TestDTO
```



## build.gradle에 추가 설정

**`build.gradle`에 아래와 같은,
`annotationProcessor` 설정을 추가**해주자.

```
dependencies {
    compile('org.projectlombok:lombok:1.18.22')
    annotationProcessor('org.projectlombok:lombok')
}
```

**여기까지 설정**했다면 **이제 `Compile` 상에서도 Error없이**

**반가운 아래 화면을 맞이**할 수 있다.

![](https://images.velog.io/images/gillog/post/c699a812-d965-4320-8f0c-6e461d6e59fe/image.png)

🙆‍♂️