---
title: "Kotlin CrossPlatform App 개발 세팅"
last_modified_at: 2023-08-14T08:21
categories:
  - kotlin
tags:
  - kotlin
  - CrossPlatform
  - MultiPlatform
toc: true
toc_label: "목차"
toc_icon: "sort"
toc_sticky: true
---

# Kotlin Multiplatform 

`Kotlin`은 `JetBrains`에서 개발한 최신 프로그래밍 언어로, 

Android 앱 개발에 있어서 `Java`를 대체하며 큰 인기를 얻고 있다. 


처음에는 Java를 대체할 `JVM(Java Virtual Machine)` 위에서 동작하는 언어로 시작되었지만, 

이후 `Kotlin/Native`와 `Kotlin/JS`를 통해 다양한 플랫폼에서도 작동을 지원한다.


![](https://kotlinlang.org/lp/multiplatform/static/multiplatform-diagram-d716356ba4b4f2488c98714db033bd53.svg)


아직 Beta 단계이지만 [Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform-mobile-getting-started.html)을 활용해 iOS, Android, macOS, Windows, Linux, watchOS

에서 Kotlin App 구동을 지원하고 있다


오늘은 이 `Kotlin Multiplatform` 환경 설정을 진행해볼 예정이다.


---

# Compose Multiplatform 설치

[Compose Multiplatform](https://github.com/JetBrains/compose-multiplatform)은 Jetpack Compose을 기반으로 한,

Kotlin의 Multiplatform간에 UI 프레임워크이다.



![](https://github.com/JetBrains/compose-multiplatform/raw/master/artwork/readme/apps.png)


`Jetpack Compose`를 기반으로하여, Android는 Stable하지만, iOS는 아직 Alpha 단계라고 한다.

iOS, Android 개발 환경 설정이 목표라 [Compose Multiplatform Mobile App](https://github.com/JetBrains/compose-multiplatform-ios-android-template/#readme) 설치를 진행한다


## Required Environment 설치

본인은 MacOS 기준에서 설치를 진행함을 밝히고,

`Compose Multiplatform Mobile App` 설치를 위해 아래 4가지 필수 환경요소들을 먼저 설치한다.

- [Xcode](https://apps.apple.com/us/app/xcode/id497799835)
- [Android Studio](https://developer.android.com/studio)
- [Kotlin Multiplatform Mobile plugin](https://plugins.jetbrains.com/plugin/14936-kotlin-multiplatform-mobile)
- [CocoaPods dependency manager](https://kotlinlang.org/docs/native-cocoapods.html#set-up-an-environment-to-work-with-cocoapods)

## kdoctor로 확인


위 4 가지 환경요소 설치 후, 아래 [KDoctor](https://github.com/Kotlin/kdoctor)를 명령어로 설치해,

필수 환경들이 잘 설치되었는지 검사한다.



```bash
$ brew install kdoctor
```

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/951f6bea-8acb-4c3b-bfc9-10ce3901f225)



```bash
$ kdoctor
```

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/a5d3e1a9-ceab-4ca5-a3d2-262e4b37de39)

_미설치된 환경이 존재할 경우 위 사진 처럼 표시 및 설치 안내 제공_


![](https://github.com/gil-log/gil-log.github.io/assets/48559894/a6b9b56d-4c80-40dd-b7a5-be120d230bde)

---

# CrossPlatform App init


이제 필수 환경설정을 마쳤으니, CrossPlatform App을 생성해본다.


## Android Studio

먼저 설치한 `Android Studio`를 실행한다.

그 후 `New Project`에서 `Kotlin Multiplatform App`으로 신규 Project를 생성한다.

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/bacaef31-162b-430a-bd80-cdd59550a1d6)


생성할 프로젝트 Application의 이름을 지정하고 Next로 생성해준다.

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/fda313c1-b0dc-4f41-afe0-ba70a73be5be)

마지막으로 `Kotlin Multiplatform App`에서 Android, iOS에서 사용할 Application Name을 지정하고 마무리한다.

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/70b8928e-b311-4261-94a1-1f6460a35978)


## Project Structure

생성된 Project에는 `androidMain`, `commonMain`, `iosMain` 세 디렉토리가 존재하고,



![](https://github.com/gil-log/gil-log.github.io/assets/48559894/ebc1c0e7-55f3-4357-b286-95fbbc573d16)


![](https://github.com/gil-log/gil-log.github.io/assets/48559894/6717a90b-a84e-478d-8af8-504c2cb720ea)


각 Directory 별로 `commonMain : Kotlin Cross Platform Main`, `androidMain : android Platform Main`, `iosMain : ios Platform Main`

기능을 수행한다.



---


# Run


이제 생성한 Project를 Android, IOS Device에서 실행해본다.

두 Platform 모두 `Android Studio` 에서 진행한다

## Android

기본적으로 별도의 설정 없이 바로 Android Studio에서 `Device Manager`에 Android Device를 추가해 실행시킬 수 있다.

_애초에 Kotlin, Multiplatform Framework의 기반인 Jetpack Compose는 Android 용이므로_

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/f8496c40-6202-4a51-8a08-724873d4fe44)


## iOS

Android Studio에서 Project 생성 시 Multiplatform 으로 생성해,

기본적으로 iOS Run Configuration이 생성되어 있다.

![](https://github.com/gil-log/gil-log.github.io/assets/48559894/b9a99dba-e23e-4464-bcd3-93ffc810eb7b)

RunConfiguration이 지정되어 있지 않다면 위 사진과 같이 RunConfiguration을 지정한다

### Execution target 미존재 시 

최초 `XCode`를 생성한 Project의 iOSAppName 프로젝트 경로까지, 한번 Open하고 구동하면 정상적으로 확인된다 


![](https://github.com/gil-log/gil-log.github.io/assets/48559894/1793b663-9ed4-4c36-9394-e071aaaee86a)


---

여기까지 간단하게 Kotlin Multiplatform 환경 설정을 해보았다.

[공식문서](https://kotlinlang.org/docs/multiplatform-mobile-getting-started.html)를 살펴보면 조금더 자세한 설명이 확인 가능하니 참고하는 것을 추천한다.
