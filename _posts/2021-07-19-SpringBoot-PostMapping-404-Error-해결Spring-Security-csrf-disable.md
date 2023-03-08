---
title: "[SpringBoot] PostMapping 404 Error 해결(Spring Security csrf disable)"
last_modified_at: 2021-07-19T22:02
categories: 
  - 에러
tags: 
  - '404' 
  - 'POST' 
  - 'Springboot' 
  - 'csrf' 
  - 'spring security'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[Spring boot security 적용시 post 404에러](https://pooney.tistory.com/55)


# 문제 상황


![](https://images.velog.io/images/gillog/post/66777d2f-c3aa-4251-96cb-074fc3069f1d/image.png)

**Spring Boot에서 기본적인 `@PostMapping` 으로 POST 방식으로 `@Valid`를 테스트** 하던 중

![](https://images.velog.io/images/gillog/post/eb8e2ac7-4bda-4350-9ea6-52786201e239/image.png)

**404 Error**를 내뿜고 있었다.

# 핵심 문구

나는 **`Spring Boot`, `Post Method`, `404 Not Found`로 구글님께 간절히** 빌었고,

**`Spring Security` Config와 관련있다는걸 발견**했다.
_간절함은 언제나 통하는 법_

# 문제 원인

원인은 **`Spring Security`는 기본적으로 `CSRF`를 막기 위해 활성화** 되어있는데,

**`@GetMapping`시에는 문제 없다가, `@PostMapping`시에는 404 Error를 내뿜는 것**이었다.

_**CSRF : Cross-Site Request Forgery(사이트 간 요청 위조) 공격자가 수정, 삭제 등의 작업으로 사용자의 의지와 무관하게 공격자의 의도대로 Application을 공격하는 취약점**_

# 문제 해결


**`Spring Security` Configure Class**에서 **`HttpSecurity`를 매개 변수로 받는 `configure` method**에 아래와 같이,

**`csrf().disable()`을 추가**해준다.

```java
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    
    private UserService userService;
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception {
        httpSecurity.authorizeRequests()
            .antMatchers("/admin").hasRole("admin")
            .antMatchers("/user").hasRole("user")
            .antMatchers("/test").permitAll()
            .and()
            .formLogin()
            .loginPage("/user/login")
            .defaultSuccessUrl("/user/result")
            .permitAll()
            .and()
            .logout()
            .logoutRequestMatcher(new AntPathRequestMatcher("/user/logout"))
            .logoutSuccessUrl("/")
            .invalidateHttpSession(true)
            .and()
            .exceptionHandling()
            .accessDeniedPage("/user/denied");
        httpSecurity.csrf().disable();
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.userDetailsService(userService).passwordEncoder(passwordEncoder());
    }

}
```

![](https://images.velog.io/images/gillog/post/b6c6fb92-8eed-47f5-98e9-23d3b5c9bf08/image.png)

성공적으로 Post Method로 Request를 사용할 수 있다! :)

**오늘도 해결 완료! 🙆🏻‍♂️**