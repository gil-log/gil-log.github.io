---
title: "[Spring] Response Object JSON Null 값 안보내는 방법(message-converters)"
last_modified_at: 2021-07-09T00:48
categories: 
  - spring
tags: 
  - 'MessageConverter' 
  - 'ObjectMapper' 
  - 'Spring' 
  - 'json'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[SPRING RESPONSE OBJECT 값 중에서 NULL 인 값은 안보내는 방법[YENA WORLD]](https://yenaworldblog.wordpress.com/2018/04/18/spring-response-object-%EA%B0%92-%EC%A4%91%EC%97%90%EC%84%9C-null-%EC%9D%B8-%EA%B0%92%EC%9D%80-%EC%95%88%EB%B3%B4%EB%82%B4%EB%8A%94-%EB%B0%A9%EB%B2%95/)


# Message-Converters


**`JSON`으로 `ResponseBody`를 통해 `JSON`으로 응답**을 보낼때,

**기본적으로 NULL 값인 요소들도 함께 전송**된다.

![](https://images.velog.io/images/gillog/post/c1482309-90ba-43d1-9a51-b38f8e1f93db/image.png)

`NULL`이 아닌 요소들만 Response로 전달하고 싶을 경우,

**`servlet-context.xml`(Spring Dispatcher Server 관련 xml)에 `message-converters`를 추가**해주면 된다.

```
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="com.fasterxml.jackson.databind.ObjectMapper">
                    <property name="serializationInclusion">
                        <value type="com.fasterxml.jackson.annotation.JsonInclude.Include">NON_NULL</value>
                    </property>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

그럼 **`NULL`이 아닌 요소만 `Response`로 전송**할 수 있다.

![](https://images.velog.io/images/gillog/post/2c958875-50bd-4a8e-b139-04c34448778e/image.png)

