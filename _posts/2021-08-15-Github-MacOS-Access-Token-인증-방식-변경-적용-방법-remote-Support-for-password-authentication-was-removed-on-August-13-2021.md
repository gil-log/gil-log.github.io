---
title: "[Github, MacOS] Access Token 인증 방식 변경 적용 방법 , (remote: Support for password authentication was removed on August 13, 2021.)"
last_modified_at: 2021-08-15T07:29
categories: 
  - macos
tags: 
  - 'MacOS' 
  - 'Please use a personal access token instead' 
  - 'access token' 
  - 'github'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️




[맥 OS: git push에서 The requested URL returned error: 403 해결 방법[카레유]](https://curryyou.tistory.com/403)


---


**2021-08-13 일자로 Git Authentication 방식이 변경** 되었다.


![](https://images.velog.io/images/gillog/post/c7a365be-d228-4f44-8b0b-66005925d29c/image.png)
>[github.blog token-authentication-requirements-for-git-operations/](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)


하여 **`git push`, `git merge`, `git pull`** 등 **Github 원격 `Origin`에 접근하려고 할때 아래와 같은 에러가 발생**한다.



```
gillog@Gillogui-MacBookAir DDaJa % git push
remote: Support for password authentication was removed on August 13, 2021. 
Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: unable to access 'https://github.com/swgil007/DDaJa.git/': The requested URL returned error: 403
```

이는 **`Git hub`에서 `personal access token`을 발급해주어 적용하면 되는데**,

해당 글은 **`MacOS`를 기준으로 작성**하였다.


---

# Github Personal Access Token 발급 받기

먼저 **`Github`에 로그인 한 후 오른쪽 상단의 자기 프로필 사진을 클릭한 후 `Settings`에 들어간다.**

![](https://images.velog.io/images/gillog/post/e5830608-996c-4aaa-b087-7064da6ae314/image.png)


그 후 **`Developer settings`를 클릭**한다.


![](https://images.velog.io/images/gillog/post/6e94dde7-9647-4f34-b3c5-2a764cc7d8fb/image.png)

**`Personal access tokens` 를 클릭하고 `Generate new token`을 클릭**한다.

![](https://images.velog.io/images/gillog/post/1a8a5d91-0dff-4fa4-bf36-fbb0ad7ec878/image.png)


해당 **`Personal access token`의 만기 일자나 설정 가능 허용 범위를 선택**해준다.

![](https://images.velog.io/images/gillog/post/15737bce-1082-438c-ac2c-ad8f2ee7c213/image.png)


그 후 **`Generate token`을 클릭**한다.

![](https://images.velog.io/images/gillog/post/976a6036-0092-4f65-9c15-d082ab80c554/image.png)


이렇게 하면 **`Personal Access Token` 발급이 완료**되었다.
![](https://images.velog.io/images/gillog/post/5a0720b4-bf1b-4399-8a5e-6e35acf732d3/image.png)


# Personal Access Token 적용하기


이제 **`Window`나 `MacOS`에서 설정 방법**이 각기 다른데,

**`MacOS`기준**으로 **`keychain`에 등록**해주면 된다.

<br>

**`Spotlight`에서 `keychian`을 검색하여 클릭**해준다.

![](https://images.velog.io/images/gillog/post/ad8491d9-a92a-4324-8a6a-e884757bc81c/image.png)


**`keychain`에서 `github`를 검색**해주고,

**등록된 아이디, 패스워드에서 패스워드**를,

**아까전 발급 받은 `Personal Access Token`으로 변경**해주면 된다.

![](https://images.velog.io/images/gillog/post/5b84543d-8bf0-4818-aaa1-bc54ea066ccf/image.png)

![](https://images.velog.io/images/gillog/post/4cffb37d-28bf-48b8-8b17-842a4a7ab7f0/image.png)


**이렇게 변경하면 정상적으로 git origin에 접근할 수 있다.**

![](https://images.velog.io/images/gillog/post/327440d0-204f-45d7-8074-75edec4753dc/image.png)