---
title: "[MySQL] Workbench Export하기 (Version 호환 문제 해결)"
last_modified_at: 2020-12-04T00:05
categories: 
  - database
tags: 
  - 'DB Export' 
  - 'DB Import' 
  - 'mysql'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
개발 서버에서 Local DB Server를 사용하다가 DB Server를 이관할때나 DB를 옮길때 MySQL에서 Export하는 방법에 대해서 진행해보려한다.
![](https://images.velog.io/images/gillog/post/0519679b-8483-4799-9a40-9d08471698b7/1.png)
먼저 `MySQL Workbench`를 열고 DB에 Connect 후 `Server` > `Data Export`를 누르자.



![](https://images.velog.io/images/gillog/post/95781485-d156-4536-a32d-351154432ca2/2.png)

그러면 위와 같이 Export할 Schema랑 Table을 선택 할 수 있다.

Export할 Table들을 확인한 후 아래의 `Start Export` 버튼을 누른다.

![](https://images.velog.io/images/gillog/post/78b45414-ab1b-4a18-9d97-c3972c9e333d/3.png)

이때 MysqlDump의 버전이 달라 오류가 발생할 수 있다는 메시지가 뜨는데 이때 `Continue Anyway`를 누르면 아래와 같이 진행된다.

![](https://images.velog.io/images/gillog/post/bf02bc37-e994-4948-9499-73ee6f54ca9e/4.png)

```
Operation failed with exitcode 2
06:49:47 Export of C:\Users\ProfessorG\Documents\dumps\Dump20201204 (1) has finished with 20 errors
```

에러와 함께 모든 Table들의 Export 작업이 중단되었다.

해결 방법을 알아보니 `mysqldump.exe`을 버전에 맞게 설치해서 경로 설정을 해준 후 Export를 진행하면 된다.

![](https://images.velog.io/images/gillog/post/93da4a35-ef62-4d87-94f9-6c5ca8b3e201/5.png)

먼저 >>[MySQL Archives](https://downloads.mysql.com/archives/community/)<<에서 버전에 맞는 Server 파일은 찾아 준다.

![](https://images.velog.io/images/gillog/post/1eca7cf0-9b76-4f33-9ec7-31ea70a5a023/6.png)

![](https://images.velog.io/images/gillog/post/5bd63825-b014-469a-8361-74e465bfbafa/7.png)

설치할 버전을 지정해 준 후 Zip파일을 운영체제에 맞게 설치해준다.

![](https://images.velog.io/images/gillog/post/175cb7db-95da-48d7-8b5d-cb16776ffc65/8.png)

이제 설치한 Zip파일을 `C:\Program Files\MySQL`에 압축 해제 해준다.

![](https://images.velog.io/images/gillog/post/c550402c-c683-48fd-8b7f-0e26f5c1bfbb/9.png)

`C:\Program Files\MySQL\설치한MySQL 버전\bin` 경로에 들어가면 `mysqldump.exe`파일이 있다.

이 경로를 설정 해주어야한다.

![](https://images.velog.io/images/gillog/post/c0a56237-db9b-4306-9dd3-963d1e3b2b18/10.png)

`MySQL Workbench` 탭 위에 `Edit > Preferences > Administration`으로 들어가면 위 `Path to mysqldump Tool` 경로를 아까전 설치한 `mysqldump.exe`경로로 설정해준다.

![](https://images.velog.io/images/gillog/post/7bbed5c6-ed93-4de4-aac8-daa38ad84634/11.png)
그 후 다시 Export를 진행하면 정상적으로 Export가 되었다.

![](https://images.velog.io/images/gillog/post/98b44c69-ca0e-436e-9429-a3f732380c40/11.png)

`Preferences > Administration`에 설정된 `Export Directory Path`에 가보면 dump된 sql 파일들을 확인할 수 있다.

<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[[MySQL]Workbench에서 테이블 Export와 Import하기[자줌마의ITWorld]](https://javagirl.tistory.com/4)

[Workbench 마이그레이션 및 Export 오류시..[거만떡볶이의 일상다반사]](https://chsoft.tistory.com/entry/Workbench-%EB%A7%88%EC%9D%B4%EA%B7%B8%EB%A0%88%EC%9D%B4%EC%85%98-%EB%B0%8F-Export-%EC%98%A4%EB%A5%98%EC%8B%9C)

[]()

[]()

[]()
