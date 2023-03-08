---
title: "[jQuery] jQuery 1.11 > 3.x Migration 하기 최종"
last_modified_at: 2021-06-09T02:11
categories: 
  - jquerymigration
tags: 
  - 'jQuery Migrate' 
  - 'jquery' 
  - 'migration'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[jQuery Deprecated](https://api.jquery.com/category/deprecated/)

[jQuery Upgrade Guide](https://jquery.com/upgrade-guide/)

[jQuery Blog](https://jquery.com/upgrade-guide/)

[jQuery Migrate](https://github.com/jquery/jquery-migrate)

<br>


---


[지난 번](https://velog.io/@gillog/jQuery-jQuery-Migrate-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)에 **`jQuery Migrate`로 프로젝트 페이지들**을 돌아다니며 **console창에서 변경해야 할 jQuery function 찾아다니며 정리**하였다.

![](https://images.velog.io/images/gillog/post/c13a0adc-f2b9-4f60-b3b3-2c1fea9fba2f/image.png)

이제 **제거되거나 Deprecated된 함수들을 변경하고 버전을 올리는 일**만 남았다.

방식은 **`Eclipse` 전체 검색으로 해당 함수들이 사용되는 곳들**을 찾고,

**권장되는 방식의 함수로 바꿔주는 방식으로 진행**했다.

이때 **다른 라이브러리나 기본 jQuery 이전 버전에서 사용되는 목록들은 검색 대상에서 제외**했다.

혹시나 **`Eclipse`에서 전체 검색의 검색 대상을 설정하고 싶은 경우 여기 >> [이클립스 검색 대상 설정[gillog]](https://velog.io/@gillog/Eclipse-%EC%A0%84%EC%B2%B4-%EA%B2%80%EC%83%89Ctrl-H-%EA%B2%80%EC%83%89-%EB%8C%80%EC%83%81-%ED%8C%8C%EC%9D%BC-%EC%A7%80%EC%A0%95%ED%95%98%EA%B8%B0-Working-Set-%EC%84%A4%EC%A0%95)에서 확인**할 수 있다.


이렇게 **아래처럼 검색해야할 함수들을 검색대상에서 검색**해주고,,, 
![](https://images.velog.io/images/gillog/post/94677983-4808-43cd-86df-bd1d0d12f59c/image.png)

`Replace`를 통해 바꿔준다..


![](https://images.velog.io/images/gillog/post/c153d4a1-f4f1-4a8e-a620-974083e7e7c8/image.png)

**`Preview`로 조심 조심 바꾸면  안되거나 잘못 바뀌는게 있나 체킹**해주고,,
_**그래도 뭔 일 날수도 있을거 같은데ㅋㅋ 대표님이 괜찮다고 그냥 하랬으니까 🙄** _
![](https://images.velog.io/images/gillog/post/166b630b-dea9-4598-b7f9-4da0c61b7ae9/image.png)

어차피 **배포본 아니니까 뭐 `OK` 눌러** 버려~


![](https://images.velog.io/images/gillog/post/d6d47acb-272e-4515-93a8-93aac2be1553/image.png)


![](https://images.velog.io/images/gillog/post/cdd1dae4-04dc-4e5e-afe5-bb5dae61b373/image.png)

그렇게 사**용하고있는 js 라이브러리에 포함된 함수들을 제외**하고,

프로젝트에서 적용 중이던 **`Deprecated` 되거나 `Removed` function들을 모두 변경**하였다.

이제 남은건,,, **사용중인 다른 jQuery를 사용하는 js 라이브러리 들 최신 릴리즈 버전** 살펴보고 **Deprecated 되는 함수들이 모두 적용되어 있으면 버전을 올리고**,

**최신 버전도 jQuery 3.x 버전대에 적용 안되어있으면 사용중인 라이브러리 소스를 수정하는 방향으로 진행**해야겠다.

![](https://images.velog.io/images/gillog/post/21cf210c-dd4e-4f3d-b66f-574e2ea86f22/image.png)

**프로젝트에서 사용 중인 jQuery 사용하는 외부 라이브러리 목록**인데 결국에는,
**jQuery 3.x 버전 이상 권장 함수 사용이 지켜진 애들이 없었다..**

**검색해서 직접 함수들을 바꿔서 적용하는 걸로 마무리**지었다.

**끝. 😀**