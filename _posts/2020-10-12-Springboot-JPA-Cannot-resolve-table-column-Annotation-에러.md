---
title: "Springboot JPA - Cannot resolve table, column Annotation 에러"
last_modified_at: 2020-10-12T14:07
categories: 
  - 에러
tags: 
  - '에러'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
Springboot에서 JPA를 사용하는데 Entity에 @Table name 옆에 빨간줄이 들어와있어서 확인해보니 Cannot resolve table 'user'라는 메시지가 떳다.

![](https://images.velog.io/images/gillog/post/5c3f7251-2bd4-4c84-abbb-128a5d3203ea/a.png)

확인해보니, InteliJ 탭에 `View > Tool Windows > Persistence` 에서 내 entityManagerFactory에 DataSource를 연관지어 주면 해결된다고 나와있었다.


![](https://images.velog.io/images/gillog/post/707b447b-3517-4913-8f2d-4ab40cd86f6f/a.png)

하지만 확인해보니 `Deafult or no data source`라고 나왔다.
Data Source란이 공백이었고 DataSource를 추가해주고 entityManagerFactory에 연관지어주면 결국 최종 해결 된다는 결론을 가졌다.

`View > Tool Windows > Database` 에 접속해서


![](https://images.velog.io/images/gillog/post/baf50ed4-6238-4588-b22f-b46e53ba5398/b.png)

`MySQL`을 선택해주고,

![](https://images.velog.io/images/gillog/post/d2457934-5f17-4543-ae71-1a9340b5b2b0/d.png)

실제 DB와 관련된 접속을 설정 해주면 된다.

`Test Connection`으로 접속을 테스트 해보니 별 이상이 없었다!



![](https://images.velog.io/images/gillog/post/44cad4d4-60bb-458a-8dd8-38ede0364b08/e.png)

성공적으로 DataSource를 추가했고,


![](https://images.velog.io/images/gillog/post/781853f7-81ed-4fdd-98f9-d061f1de024f/f.png)

![](https://images.velog.io/images/gillog/post/850d16a9-94bc-4762-a470-a4464574e99c/h.png)

결국 `Persistence` 메뉴에서 해당 프로젝트 `Assgin Data Sources` 에서 DataSource를 추가해주었다.



![](https://images.velog.io/images/gillog/post/d2ea58f4-6a8f-4aa1-948b-912a2a14b73b/ggggg.png)


해결 완료!
