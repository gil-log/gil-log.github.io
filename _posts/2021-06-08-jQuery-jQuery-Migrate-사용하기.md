---
title: "[jQuery] jQuery Migrate 사용하기"
last_modified_at: 2021-06-08T04:32
categories: 
  - jquerymigration
tags: 
  - 'jQuery Migrate' 
  - 'jquery' 
  - '마이그레이션'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[jQuery Migrate[jQuery Github]](https://github.com/jquery/jquery-migrate)

<br>

---

# jQuery Migrate

**`jQuery Migrate`는 `jQuery`에서 제공하는 Migration Tool**로써,

**jQuery의 주요 변경 사항이 도입**되었을 때, 

**제거 된 jQuery API를 복원**하여 **Migration을 더 쉽게** 만들고,

`removed`되거나 `deprecated`된 API를 사용할 때 브라우저 `console`에 경고를 표시해준다.

이를 통해 **필요하지 않고 제거 할 수 있는 발생 가능한 오류를 발견하고 수정할 수 있다.**

# How To Use?


먼저 **사용하고 있는 `jQuery`의 version**과 **Migration의 Target Version에 따라** 다른데,

결론적으로 **`jQuery 1.9` 이하 버전을 사용 중**이라면,

먼저 **`jQuery Migrate 1.x`를 통해 `jQuery 1.12.x`나 `jQuery 2.2.x`버전 대로 업그레이드**를 먼저 진행 해야한다.

**`jQuery 1.12.x`, `jQuery 2.2.x` 버전 이상**만 **`jQuery Migrate 3.x`를 통해 `jQuery 3.x` 버전 대로 Migration을 진행**할 수 있다.

<br>

그 후 **테스트를 원하는 jQuery를 사용 중인 Front 화면 단에 cdn으로 `jQuery Migrate`를 추가**한다.
_[EX] JSP_


# jQuery Migrate Version

<br>

**`jQuery 1.12.x` or `jQuery 2.2.x`이상 버전을 사용중이고 `jQuery 3.x` 이상으로 Upgrade** 하는 경우
> 
`<script src="https://code.jquery.com/jquery-migrate-3.3.2.js"></script>`

**`jQuery 1.9`이하 버전을 사용중**이고, **`jQuery 1.12.x` or `jQuery 2.2.x`로 먼저 Upgrade** 필요할 경우

> `<script src="https://code.jquery.com/jquery-migrate-1.4.1.js"></script>`




그러면 이제 아래 처럼, `jQuery Migrate`의 Version에 해당하는 `jQuery`에서 **Upgrade 해야 하는 내용들을 `console`에서 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/69394f00-8114-46c6-a75f-2dec24d97843/image.png)


**`console`에서 뜬 `jQuery Migrate` 알림 메세지**는

**이곳 >> [jQuery Migrate Github Warings.md](https://github.com/jquery/jquery-migrate/blob/main/warnings.md)에서 자세한 내용을 확인**할 수 있다.

![](https://images.velog.io/images/gillog/post/6f9b9765-a569-4821-a648-86ce02f51837/image.png)