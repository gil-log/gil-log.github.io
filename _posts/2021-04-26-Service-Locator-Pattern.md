---
title: "Service Locator Pattern"
last_modified_at: 2021-04-26T11:36
categories: 
  - 자바성능튜닝이야기
tags: 
  - 'Design Pattern' 
  - 'Service Locator' 
  - '디자인 패턴' 
  - '패턴'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>



# Service Locator 패턴

**`Service Locator 패턴`**은 예전에 많이 사용 되었던 **`EJB`의 `EJB Home 객체`나 DB의 DataSource를 lookup, 즉 찾을 때 소요되는 응답 속도 감소를 위해 사용** 된다.

`Service Locator`의 예시는 아래 코드와 같다.


```Java
public class ServiceLocator {
    private InitialContext ic;
    private Map cache;
    private static ServiceLocator serviceLocator;
    static {
    	serviceLocator = new ServiceLocator();
    }
    private ServiceLocator() {
    	cacheMap = Collections.synchronizedMap(new HashMap());
    }
    public InitialContext getInitalContext() throws Exception {
    	try {
        	if (ic == null) {
            	ic = new InitialContext();
            }
        } catch (Exception e) {
        	throw e;
        }
        return ic;
    }
    public static ServiceLocator getInstance() {
    	return serviceLocator;
    }
    
    public EJBLocalHome getLocalHome (String jndiHomeName) throws Exception {
    	EJBLoclaHome home = null;
  	try {
   		if (cache.containsKey(jndiHomeName)) {
    		home = (EJBLocalHome)cache.get(jndiHomeName);
   		} else {
    			home = (EJBLocalHome)getInitialContext().lookup(jndiHomeName);
    			cache.put(jndiHomeName, home);
   		}
  	} catch (Exception e) {
   		throw new Exception(e.getMessage());
  	}
  	return home;
    }
}
```

**`Service Locator` 패턴은 `cache`이라는 Map 객체에 home 객체를 찾은 결과를 보관하여 저장**	한다.

먼저 **`cache`에서 해당 객체를 찾고 존재하지 않으면 메모리에서 해당 객체를 찾는 방식**이다.

**`EJB home 객체`를 찾거나 `Datasource 객체`를 찾는 시간을 일반적으로 30ms 가 소요** 된다.

**`EJB Remote home`을 찾을 때에 시스템에 따라 1초 이상 소요되는 시스템도 있다.**

하지만 **메모리에서 찾으면 0.01 ms 도 소요되지 않으므로 적어도 100배 이상의 성능 향상**을 가져 올 수 있다.