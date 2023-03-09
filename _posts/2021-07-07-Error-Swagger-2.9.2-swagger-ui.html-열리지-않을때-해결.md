---
title: "[Error] Swagger 2.9.2 /swagger-ui.html 열리지 않을때 해결"
last_modified_at: 2021-07-07T01:54
categories: 
  - error
tags: 
  - 'Spring' 
  - 'Spring Fox' 
  - 'Swagger'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 상황


**`pom.xml`에 Spring Swagger 2.9.2 관련 Dependency를 추가**하고,

**`SwaggerConfig` 관련 Class를 생성하여 Server 실행 시 정상 동작**하지만,

**`localhost/swagger-ui.html` 로 Swagger 문서 접속이 안되는 상황**이었다.


```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
	@Bean
	public Docket api() {
		return new Docket(DocumentationType.SWAGGER_2).consumes(getConsumeContentTypes())
				.produces(getProduceContentTypes())
				.useDefaultResponseMessages(false)
				.apiInfo(getApiInfo())
				.select()
				.apis(RequestHandlerSelectors.basePackage("("****************")"))
				.paths(PathSelectors.ant("/**"))
				.build();
	}

	private Set<String> getConsumeContentTypes() {
		Set<String> consumes = new HashSet<>();
		consumes.add("application/json;charset=UTF-8");
		consumes.add("application/x-www-form-urlencoded");
		return consumes;
	}

	private Set<String> getProduceContentTypes() {
		Set<String> produces = new HashSet<>();
		produces.add("application/json;charset=UTF-8");
		return produces;
	}

	private ApiInfo getApiInfo() {
		return new ApiInfoBuilder().title("("****************")")
				.description("****************")
				.contact(new Contact("("****************")", "("****************")", "("****************")"))
				.version("1.0")
				.build();
	}
}
```

# 해결

**`@EnableSwagger2`를 통해 `Swagger Config`를 담당하는 Class**에 **`WebMvcConfigurationSupport`를 상속**받은 후,

**`addResourceHandlers` method를 override** 해주면 된다.



```java

@Configuration
@EnableSwagger2
public class SwaggerConfig extends WebMvcConfigurationSupport {
	@Bean
	public Docket api() {
		return new Docket(DocumentationType.SWAGGER_2).consumes(getConsumeContentTypes())
				.produces(getProduceContentTypes())
				.useDefaultResponseMessages(false)
				.apiInfo(getApiInfo())
				.select()
				.apis(RequestHandlerSelectors.basePackage("("****************")"))
				.paths(PathSelectors.ant("/**"))
				.build();
	}

	private Set<String> getConsumeContentTypes() {
		Set<String> consumes = new HashSet<>();
		consumes.add("application/json;charset=UTF-8");
		consumes.add("application/x-www-form-urlencoded");
		return consumes;
	}

	private Set<String> getProduceContentTypes() {
		Set<String> produces = new HashSet<>();
		produces.add("application/json;charset=UTF-8");
		return produces;
	}

	private ApiInfo getApiInfo() {
		return new ApiInfoBuilder().title("("****************")")
				.description("****************")
				.contact(new Contact("("****************")", "("****************")", "("****************")"))
				.version("1.0")
				.build();
	}
    
	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/swagger-ui.html").addResourceLocations("classpath:/META-INF/resources/");
		registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
		super.addResourceHandlers(registry);
	}
}
```
