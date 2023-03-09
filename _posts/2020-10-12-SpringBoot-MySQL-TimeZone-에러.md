---
title: "SpringBoot, MySQL TimeZone 에러"
last_modified_at: 2020-10-12T13:44
categories: 
  - error
tags: 
  - '에러'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
SpringBoot에서 MySQL을 사용하는데 서버 실행에 에러가 생겼다.

에러 문구를 살펴보니

![](https://images.velog.io/images/gillog/post/65b34d11-d8ba-4d60-97c7-708b86df0750/2.png)

```
Caused by: com.mysql.cj.exceptions.InvalidConnectionAttributeException: The server time zone value '���ѹα� ǥ�ؽ�' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the 'serverTimezone' configuration property) to use a more specifc time zone value if you want to utilize time zone support.
```

TimeZone과 관련된 이슈라고 나와있었다.

`application.properties`에서 MySQL 연동 관련된 곳을 찾아보니

![](https://images.velog.io/images/gillog/post/81510278-dcd0-44dc-9cb9-cec1d03be637/1.png)

위 처럼 TimeZone 설정이 없어서 생긴 오류 였다.

`spring.datasource.url` 뒤에 `serverTimezone=KST`을 붙여주어서 해결해보려 했지만

![](https://images.velog.io/images/gillog/post/c82cd9a2-eeb9-4e28-a534-c44f1491570f/4.png)

또 똑같이 오류가 떳다!

![](https://images.velog.io/images/gillog/post/ea96f14d-36e9-4713-975b-a3d3ac6f3f18/5.png)

확인해보니 KST 시간대를 매핑할 수 없다는 문제였다.

KST 말고 Asia/Seoul로는 우리 나라 시간대로 매핑 할 수 있다는 걸 확인하고


![](https://images.velog.io/images/gillog/post/d854e799-3ab3-4c26-992c-b5e6cc9bad78/1241412.png)

결국 **최종적으로는 `spring.datasource.url` 뒤에 `serverTimezone=Asia/Seoul` 을 붙여서 해결**했다.


