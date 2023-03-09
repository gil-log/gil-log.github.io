---
title: "SpringBoot - MySQL Connection 문제 해결"
last_modified_at: 2020-10-19T13:54
categories: 
  - error
tags: 
  - '에러'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
노트북에서 개발하던 토이프로젝트를 집 컴퓨터에서 Pull 받고 다시 개발 하려는데 아래와 같은 에러가 발생했다.

![](https://images.velog.io/images/gillog/post/12df0b2a-287d-4aa0-b449-bfd56be4709a/1.png)

```
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jpaMappingContext': Invocation of init method failed; nested exception is javax.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.exception.JDBCConnectionException: Unable to open JDBC Connection for DDL execution

```



확인해보니 MySQL Connection이 안되고 있는 것 같아 cmd에서 MySQL이 실행 되는지 확인해봤다.

![](https://images.velog.io/images/gillog/post/31e97170-584c-46fb-9100-630d45b6663c/2.png)

```
ERROR 2003 (HYOOO): Cant't connect to MySQL server on 'localhost' (10061)
```

cmd에서 `mysql -u root -p`명령어로 root 계정에 접속을 시도해 봤지만, 역시 연결 할 수 없다고 나왔다.

![](https://images.velog.io/images/gillog/post/2cd4d5e7-36a0-4654-a00d-27c2775c63f4/3.png)

`netstat -a`로 확인해보니 MySQL 포트에 해당하는 3306이 실행 상태가 아닌걸 발견했다.


![](https://images.velog.io/images/gillog/post/a4db90ad-2831-4653-850b-2b012040e9bf/4.png)

윈도우 + R키에 `services.msc`를 입력해 서비스 관리 창으로 이동했다.

![](https://images.velog.io/images/gillog/post/c5932c4f-818f-4d56-adf0-7d6578d87b7e/5.png)


`MySQL80` 항목을 찾아간 후 우클릭 > 실행을 시켜주었다.

![](https://images.velog.io/images/gillog/post/c7f88c39-cede-4923-a225-8188ad138b86/6.png)



![](https://images.velog.io/images/gillog/post/46f97429-2d57-4c4c-b5da-03ddde3fe319/7.png)


cmd창에서 root 계정에 접속해보니 이번엔 접속이 성공했다.

![](https://images.velog.io/images/gillog/post/db9ce91f-0c6d-41aa-a9df-945e9ce591d3/8.png)

다시 SpringBoot를 실행시키니 성공적으로 실행 되었다. 


해결완료!

