---
title: "No qualifying bean of type, 생성자 주입 방식 @Resource(name) 에러"
last_modified_at: 2020-11-07T08:49
categories: 
  - error
tags: 
  - '에러'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
```
 2020 5:34:36 오후 org.apache.catalina.core.StandardWrapperValve invoke
SEVERE: Allocate exception for servlet [DispatcherServlet]
org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type [kr.happyjob.chainmaker.basic.repo.UserDAO] is defined: expected single matching bean but found 2: UserDAOEventImpl,UserDAOImpl
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveNotUnique(DependencyDescriptor.java:172)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1059)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1018)
	at org.springframework.beans.factory.support.ConstructorResolver.resolveAutowiredArgument(ConstructorResolver.java:834)
	at org.springframework.beans.factory.support.ConstructorResolver.createArgumentArray(ConstructorResolver.java:741)
```

다음과 같은 에러가 발생 했다.

현재 상황은

Controller > Service > Repository로 가는 중에

**@Autowired 방식이 아닌 @Resource Annotation을 통한 DI를 테스트 해보던 중에 발생**했다.


- BasicController

```java

@Controller
@RequestMapping(value = "/basic")
public class BasicController {
	
	private static final Logger logger = LoggerFactory.getLogger(BasicController.class);
	
	@Resource(name="UserServiceImpl")
	private UserService userService;
	
	// MVC 기본 구조
	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String mvcBasic(Model model) {
		logger.info("GilContorller.mvcBasic() 실행 중");
	
		String anyLetterCanBeHere = "테스트 중 입니다.";
		
		String loginID = "admin";
		
		UserDTO userDTO = userService.getUserInfoByLoginID(loginID);
		
		String name = userDTO.getName();

		model.addAttribute("message", anyLetterCanBeHere);
		model.addAttribute("loginID", loginID);
		model.addAttribute("name", name);
		
		model.addAttribute("userDTO", userDTO);
		
		return "basic/mvc_basic";
	}
	
	// Vue 관련 기본 예제
	@RequestMapping(value = "/vue", method = RequestMethod.GET)
	public String vueBasic(){

		logger.info("GilContorller.vueBasic() 실행 중");
		
		return "basic/vue_basic";
	}
	
	// Vue dataTable 예제
	@RequestMapping(value = "/datatable", method = RequestMethod.GET)
	public String vueDataTable(){

		logger.info("GilContorller.vueDataTable() 실행 중");
		
		return "basic/vue_datatable";
	}
}

```

- UserServiceImpl
```java
@Service("UserServiceImpl")
@Transactional
@AllArgsConstructor
public class UserServiceImpl implements UserService {

	@Resource(name="UserDAOEventImpl")
	UserDAO userDAO;
	
	@Override
	public UserDTO getUserInfoByLoginID(String loginID) {
		
		UserVO userVO = userDAO.getUserInfoByLoginID(loginID);
		
		UserDTO userDTO = new UserDTO(userVO);
		
		return userDTO;
	}

}
```

- UserDAOEventImpl


```java
@Repository("UserDAOEventImpl")
@AllArgsConstructor
public class UserDAOEventImpl implements UserDAO{

	private SqlSession sql;
	
	@Override
	public UserVO getUserInfoByLoginID(String loginID) {
		return sql.selectOne("userMapper.getUserInfoByLoginID", "babo");
	}
    }

  ```

- UserDAOImpl
  
```java

@Repository("UserDAOImpl")
@AllArgsConstructor
public class UserDAOImpl implements UserDAO{

	private SqlSession sql;
	
	@Override
	public UserVO getUserInfoByLoginID(String loginID) {
		return sql.selectOne("userMapper.getUserInfoByLoginID", loginID);
	}

}
```


각각의 Stereotype Annotation에 name을 배당해주었는데도

```
No qualifying bean of type [kr.happyjob.chainmaker.basic.repo.UserDAO] is defined: 
expected single matching bean but found 2: UserDAOEventImpl,UserDAOImpl
```

UserServiceImpl에서 UserDAO에 UserDAOEventImpl **bean name으로 매핑 시켜주는 부분에서 문제가 발생하는걸 확인**했다.

- 문제 부분
```java
@Service("UserServiceImpl")
@Transactional
@AllArgsConstructor
public class UserServiceImpl implements UserService {

	@Resource(name="UserDAOEventImpl")
	UserDAO userDAO;
	
	@Override
	public UserDTO getUserInfoByLoginID(String loginID) {
		
		UserVO userVO = userDAO.getUserInfoByLoginID(loginID);
		
		UserDTO userDTO = new UserDTO(userVO);
		
		return userDTO;
	}

}
```

결국 여러가지 시도를 벌이다가

**`@AllArgsConstructor`를 지우면서 해결** 할 수 있었다.


- 해결 부분

```java
@Service("UserServiceImpl")
@Transactional
public class UserServiceImpl implements UserService {

	@Resource(name="UserDAOEventImpl")
	UserDAO userDAO;
	
	@Override
	public UserDTO getUserInfoByLoginID(String loginID) {
		
		UserVO userVO = userDAO.getUserInfoByLoginID(loginID);
		
		UserDTO userDTO = new UserDTO(userVO);
		
		return userDTO;
	}

}
```

아마도 원인은 @AllArgsConstructor는 생성 당시에 필드 값을 파라미터로 받은 생성자를 추가해주는 Lombok Annotation인데 Container에서 streotype component들을 bean 생성할때 각각의 UserDAOImpl, UserDAOEventImpl에서 @Repositroy에 선언한 bean name을 @Resource 부분에서 매핑을 못시켜줘서 발생한 오류인것 같다.

더 자세히 알아봐야겠다.

어쨋든 해결 완료
