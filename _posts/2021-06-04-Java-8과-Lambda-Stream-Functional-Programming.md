---
title: "Java 8과 Lambda, Stream, Functional Programming"
last_modified_at: 2021-06-04T00:24
categories: 
  - modernjavainaction
tags: 
  - 'Modern Java In Action' 
  - 'functional programming' 
  - 'lambda' 
  - 'stream'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[Modern Java in Action](http://www.kyobobook.co.kr/product/detailViewEng.laf?ejkGb=BNT&mallGb=ENG&barcode=9781617293566)


---

# Lambda

**`Lambda`의 핵심은 간결하게 Java Program을 구현 할 수있다는 것**이다.

**`Lambda`를 사용하면 `Event Handler`, `CallBack`등을 더 다양한 분야에서 사용**할 수 있다.

다시 말해 **`Lambda`와 Method 참조를 통해 어떤 코드 동작 중에 실행** 될 수 있는 **Code나 Method를 간단하게 인수로 전달** 할 수 있다.

---

# Stream

**`Stream`은 Java 8 부터 추가된 놀라운 기능**이다.

**`Collection`과 비슷하게 동작**하지만, **새로운 Programming 형식을 지원하는 훌륭한 기능**이다.

**SQL 같은 DB 질의어도 기존 Java Code로 작성하려면 많은 행이 필요**했지만, **`Stream`을 사용하면 간단하게 Programming** 할 수 있고, 

**Stream을 처리하는 Data와 처리된 Data를 포함해 모두 Memory에 저장 하지 않을 수도 있게 설계**되었다.

따라서 **`Stream`을 이용하면 Memory에 저장하기 힘든 큰 Data도 문제 없이 처리** 할 수 있다.

**`Collection`에서 처리 할 수 없는 최적화도 적용**되었는데,

**여러 수행을 Group화 하여 Data를 여러 번 탐색 없이 한 번의 탐색**으로 마칠 수 있고,
**해당 수행들을 병렬화 할 수도있다.**

---

# Functional Programming

**`Functional Programming`은 Programming 기법**으로, **`Function`을 Value로 취급**한다.

**Java 8 엄청난 변화는 `Functional Programming`의 장점을 Java 문법에 접목시켰다는 것**이다.

**Java 8의 설계 덕에 `Functional Programming`**을 **Java 8에 새롭게 추가 된 Design Pattern 처럼 사용**할 수 있고, **짧은 시간에 명확하고 간결한 Code 작성이 가능**해졌다.

