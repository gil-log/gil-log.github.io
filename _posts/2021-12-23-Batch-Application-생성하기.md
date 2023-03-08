---
title: "[Spring] Batch Application 생성하기"
last_modified_at: 2021-12-23T11:33
categories: 
  - spring
tags: 
  - 'Spring' 
  - 'batch'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# #import

[Spring Batch[docs.Spring.io]](https://docs.spring.io/spring-batch/docs/current/reference/html/index.html)

[Spring Batch[기억보단 기록을]](https://jojoldu.tistory.com/category/Spring%20Batch)

[[에러노트] Spring Batch : Unknown column 'E.JOB_CONFIGURATION_LOCATION' in 'field list'](https://developyo.tistory.com/230)

---

# Batch Application

**`Batch Application`**은 **특정 대용량 데이터 처리 등의 빈번하지 않은 비즈니스 로직**을

**사용량이 적은 시간대에 단발성으로 처리하는 `Application`을 의미**한다.
_[해당 글로 간략하게 참고](https://velog.io/@gillog/batch)_


## 조건

**`Batch Application`은 아래 다섯 가지 조건을 만족**해야 한다.


### 1. 대용량 데이터 처리 가능

**`Batch Application`**은 **대량의 데이터를 읽거나 조작**하는 등,

**특정 처리 로직을 수행할 수 있어야 한다.**

### 2. 자동화 가능

**`Batch Application`은 구동 후 개발자의 관여 없이도 스스로 실행**되며

**반복적으로 로직을 처리할 수 있어야 한다.**
_심각한 예외 상황을 제외_


### 3. 신뢰 가능

**`BatchApplication`은 로직 수행 중 문제가 발생**하였을 경우,

**`Logging`등의 형태**로 **문제가 된 부분에 대해 `Tracking`이 가능해야 한다.**


### 4. 견고한 구조

**`Batch Application`은 로직 수행 중 예외적인 데이터나 상황**에 대해

**중단 없이 처리할 수 있어야 한다.**
_예외 상황이나 충돌 상황에 대한 견고한 대비 필요_


### 5. 성능

**`Batch Application`은 대용량의 데이터를 처리**하게 되는데

이때 **예상한 Resource(시간, 메모리 등) 내에 로직 처리를 완료 하거나,**

**동시에 실행되는 다른 Application이나 비즈니스 로직의 성능**에 **방해되지 않게 실행되어야 한다.**




<br>

**Java Spring Framework를 사용하여 Web Application을 구동 중**이라면,

**`Spring Batch`를 통해 Batch Application을 생성하여 사용**할 수 있다.

오늘은 **간단한 `Spring Batch` 구성 요소와 사용 방법에 대해** 알아보려한다.

_Spring Boot, Java8, Mysql, Gradle 환경에서 예제 구동_


---

# Spring Batch

먼저 **`Spring Batch`를 사용하기 전 간단하게 구성요소와 처리 방식에 대해** 살펴보려 한다.


## 구성요소

**`Spring Batch`**는 **가장 큰 단위**로 **하나 하나 `Batch`의 단위인 `Job`**,

**`Job` 내부에서 각 로직 처리 순서부분**에 해당하는 **`Step`**

그 **`Step`안에 포함**된 **`Tasklet` 이나 [`Reader`, `Processor`, `Writer` ] 단위로 구성**된다.


**`Job`, `Step`, `Tasklet`, [`Reader`, `Processor`, `Writer`] 의 요소들**을 **그림으로 표현하면 아래**와 같다.




![](https://images.velog.io/images/gillog/post/bf3040b5-5d35-457a-92fc-c7fbec7e0e96/image.png)


**하나의 `Batch` 단위인 `Job`**은 **여러개의 `Step`으로 구성**되어있고,

이 **`Step` 안**에는 **`Tasklet` 단위로 구성되거나**,

**[`Reader`, `Processor`, `Writer`] 세 요소 묶음 형태로 구성**된다.

<br>

이때 **`Step`을 구성하는 `Tasklet`과 [`Reader`, `Processor`, `Writer`] 세 요소 묶음**은

**병렬형태로 같이 포함되어 구성될 수 없다.**
_`Reader` > `Processor` > `Tasklet` 마무리 형태 불가능_







---

## 처리 방식

**`Spring Batch`**에서 **`Job`은 하나의 `Batch` 작업 단위를 의미**한다.

**`Job`을 처리하는 과정을 살펴보면 아래 그림**과 같은데,



>![](https://images.velog.io/images/gillog/post/d91c2eca-9d79-4dcd-9bd6-4ccb60bda117/image.png)
_[by docs.spring.io - job configure](https://docs.spring.io/spring-batch/docs/current/reference/html/job.html#configureJob)_


**`Spring Batch`**에서 **`Batch` 작업(`Job`)**은 **`JobRepository`를 통해 `Instance`화** 되며

**작업(`Job`)의 실행**은 **`JobLauncher`를 통해 아래 Diagram과 같은 형태로 구동**된다.

>![](https://images.velog.io/images/gillog/post/dd97ece7-8566-4e9e-8cb5-7c3cdbb6fdb9/image.png)
_[by docs.spring.io - job configure](https://docs.spring.io/spring-batch/docs/current/reference/html/job.html#configureJob)_


**`Spring`에서 `@EnableBatchProcessing` Annotation을 사용**하면,

**아래 6가지 요소들**을 **`Bean`으로 등록해주어 개발자가 사용할 수 있게** 된다.

- `JobRepository`
- `JobLauncher`
- `JobRegistry`
- `PlatformTransactionManager`
- `JobBuilderFactory`
- `StepBuilderFactory`



---



## Spring Batch 구현 형태


앞서 살펴본 **내부 구성요소를 변경 하지 않고 사용할 때**는 


**`JobBuilderFactory`와 `StepBuilderFactory` **


**두 가지 `Bean`을 사용하여 대부분의 Batch Application을 구현**할 수 있다.


<br>

**지금까지 살펴본 `Spring Batch`의 구성 요소들**을 통해

**실제 구현할 때는 아래와 같은 코드 형태**이다.

<br>



```java
// 환경 설정 관련 Class 임을 명시
@Configuration
// Spring Batch Bean들을 주입받기 위해 사용
@EnableBatchProcessing
// JobBuilderFactory, StepBuilderFactory를 생성자 형태로 DI(의존성 삽입)
@RequiredArgsConstructor
public class GillogBatchConfig {
    // @EnalbeBatchProcessing에 의해 Bean 등록된 요소 DI
    private final JobBuilderFactory jobs;
    private final StepBuilderFactory steps;

    // 하나의 Batch 단위 Job 선언 및 Bean 등록
    @Bean
    public Job job(@Qualifier("step1") Step step1, @Qualifier("step2") Step step2) {
        return jobs.get("gillogJob").start(step1).next(step2).build();
    }

    // Job을 구성하는 Step을 [Reader, Processor, Writer] 단위로 구현 및 Bean 등록
    @Bean
    protected Step step1(ItemReader<Person> reader,
                         ItemProcessor<Person, Person> processor,
                         ItemWriter<Person> writer) {
        return steps.get("step1")
            .<Person, Person> chunk(10)
            .reader(reader)
            .processor(processor)
            .writer(writer)
            .build();
    }


    // Job을 구성하는 Step을 Tasklet 단위로 구현 및 Bean 등록
    @Bean
    protected Step step2(Tasklet tasklet) {
        return steps.get("step2")
            .tasklet(tasklet)
            .build();
    }
}
```

---
  
 # Spring Boot 구현 예제
  
먼저 **해당 예제는 `Spring Boot 2.5.6`, `Gradle`, `Java8`, `MySQL` 환경에서 구현**되었다.
  
**해당 환경의 Project가 존재한다는 가정하에 시작**한다.
  
### Spring Batch Dependency 추가
  
먼저 **`build.gradle`에 아래와 같이 `Spring Batch` Dependency를 추가**해주자.
  
```gradle
  
dependencies {

    ...
    compile('org.springframework.boot:spring-boot-starter-batch')
    testCompile('org.springframework.batch:spring-batch-test')
    
}
```
  _위 문구 추가 후 Gradle을 Compile_


### `@EnableBatchProcessing` Annotation 추가

해당 **`Spring Boot`의 프로젝트 최상단 `Application.class`**에 
_`@SpringBootApplication` Annotation 선언 Class_

**`@EnableBatchProcessing`를 추가**해주자.

```java
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@EnableBatchProcessing
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

### Spring Batch 관련 DB Table 추가

**`Spring Batch`**는 **Batch 구동 및 Instance 관련 Meta-Data Table이 필요**하다.

![](https://images.velog.io/images/gillog/post/18a76f31-68b6-4950-8702-6274458b0090/image.png)

해당 Project에 **연동한 DB Schema에 Spring Batch Meta-Data Table이 존재하지 않을 경우**,

**Project 구동시에 Error가 발생**하게 된다.

관련 **`DDL`은 각 IDE의 `파일명 검색`을 통해서 `schema-` 라는 문구를 검색**하면 찾을 수 있다.
_Intellij 기준 Window : Shift + Ctrl + N, Mac : Shift + Command + O_

![](https://images.velog.io/images/gillog/post/fee698da-470e-496d-9ff6-ce0b4d688632/image.png)

**혹시나 파일명 검색이 힘든 상황**이라면 **아래 `MySQL` 기준 아래 DDL을 사용**하자.

```sql

CREATE TABLE BATCH_JOB_INSTANCE  (
        JOB_INSTANCE_ID BIGINT  NOT NULL PRIMARY KEY ,
        VERSION BIGINT ,
        JOB_NAME VARCHAR(100) NOT NULL,
        JOB_KEY VARCHAR(32) NOT NULL,
        constraint JOB_INST_UN unique (JOB_NAME, JOB_KEY)
) ENGINE=InnoDB;

CREATE TABLE `batch_job_execution` (
  `JOB_EXECUTION_ID` bigint(20) NOT NULL,
  `VERSION` bigint(20) DEFAULT NULL,
  `JOB_INSTANCE_ID` bigint(20) NOT NULL,
  `CREATE_TIME` datetime(6) NOT NULL,
  `START_TIME` datetime(6) DEFAULT NULL,
  `END_TIME` datetime(6) DEFAULT NULL,
  `STATUS` varchar(10) DEFAULT NULL,
  `EXIT_CODE` varchar(2500) DEFAULT NULL,
  `EXIT_MESSAGE` varchar(2500) DEFAULT NULL,
  `LAST_UPDATED` datetime(6) DEFAULT NULL,
  `JOB_CONFIGURATION_LOCATION` varchar(2500) DEFAULT NULL,
  PRIMARY KEY (`JOB_EXECUTION_ID`),
  KEY `JOB_INST_EXEC_FK` (`JOB_INSTANCE_ID`),
  CONSTRAINT `JOB_INST_EXEC_FK` FOREIGN KEY (`JOB_INSTANCE_ID`) REFERENCES `batch_job_instance` (`JOB_INSTANCE_ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE BATCH_JOB_EXECUTION_PARAMS  (
        JOB_EXECUTION_ID BIGINT NOT NULL ,
        TYPE_CD VARCHAR(6) NOT NULL ,
        KEY_NAME VARCHAR(100) NOT NULL ,
        STRING_VAL VARCHAR(250) ,
        DATE_VAL DATETIME(6) DEFAULT NULL ,
        LONG_VAL BIGINT ,
        DOUBLE_VAL DOUBLE PRECISION ,
        IDENTIFYING CHAR(1) NOT NULL ,
        constraint JOB_EXEC_PARAMS_FK foreign key (JOB_EXECUTION_ID)
        references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID)
) ENGINE=InnoDB;

CREATE TABLE BATCH_STEP_EXECUTION  (
        STEP_EXECUTION_ID BIGINT  NOT NULL PRIMARY KEY ,
        VERSION BIGINT NOT NULL,
        STEP_NAME VARCHAR(100) NOT NULL,
        JOB_EXECUTION_ID BIGINT NOT NULL,
        START_TIME DATETIME(6) NOT NULL ,
        END_TIME DATETIME(6) DEFAULT NULL ,
        STATUS VARCHAR(10) ,
        COMMIT_COUNT BIGINT ,
        READ_COUNT BIGINT ,
        FILTER_COUNT BIGINT ,
        WRITE_COUNT BIGINT ,
        READ_SKIP_COUNT BIGINT ,
        WRITE_SKIP_COUNT BIGINT ,
        PROCESS_SKIP_COUNT BIGINT ,
        ROLLBACK_COUNT BIGINT ,
        EXIT_CODE VARCHAR(2500) ,
        EXIT_MESSAGE VARCHAR(2500) ,
        LAST_UPDATED DATETIME(6),
        constraint JOB_EXEC_STEP_FK foreign key (JOB_EXECUTION_ID)
        references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID)
) ENGINE=InnoDB;

CREATE TABLE BATCH_STEP_EXECUTION_CONTEXT  (
        STEP_EXECUTION_ID BIGINT NOT NULL PRIMARY KEY,
        SHORT_CONTEXT VARCHAR(2500) NOT NULL,
        SERIALIZED_CONTEXT TEXT ,
        constraint STEP_EXEC_CTX_FK foreign key (STEP_EXECUTION_ID)
        references BATCH_STEP_EXECUTION(STEP_EXECUTION_ID)
) ENGINE=InnoDB;

CREATE TABLE BATCH_JOB_EXECUTION_CONTEXT  (
        JOB_EXECUTION_ID BIGINT NOT NULL PRIMARY KEY,
        SHORT_CONTEXT VARCHAR(2500) NOT NULL,
        SERIALIZED_CONTEXT TEXT ,
        constraint JOB_EXEC_CTX_FK foreign key (JOB_EXECUTION_ID)
        references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID)
) ENGINE=InnoDB;

CREATE TABLE BATCH_STEP_EXECUTION_SEQ (
        ID BIGINT NOT NULL,
        UNIQUE_KEY CHAR(1) NOT NULL,
        constraint UNIQUE_KEY_UN unique (UNIQUE_KEY)
) ENGINE=InnoDB;

INSERT INTO BATCH_STEP_EXECUTION_SEQ (ID, UNIQUE_KEY) select * from (select 0 as ID, '0' as UNIQUE_KEY) as tmp where not exists(select * from BATCH_STEP_EXECUTION_SEQ);

CREATE TABLE BATCH_JOB_EXECUTION_SEQ (
        ID BIGINT NOT NULL,
        UNIQUE_KEY CHAR(1) NOT NULL,
        constraint UNIQUE_KEY_UN unique (UNIQUE_KEY)
) ENGINE=InnoDB;

INSERT INTO BATCH_JOB_EXECUTION_SEQ (ID, UNIQUE_KEY) select * from (select 0 as ID, '0' as UNIQUE_KEY) as tmp where not exists(select * from BATCH_JOB_EXECUTION_SEQ);

CREATE TABLE BATCH_JOB_SEQ (
        ID BIGINT NOT NULL,
        UNIQUE_KEY CHAR(1) NOT NULL,
        constraint UNIQUE_KEY_UN unique (UNIQUE_KEY)
) ENGINE=InnoDB;

INSERT INTO BATCH_JOB_SEQ (ID, UNIQUE_KEY) select * from (select 0 as ID, '0' as UNIQUE_KEY) as tmp where not exists(select * from BATCH_JOB_SEQ);
```

### Spring Batch : Unkown column .. Error 발생??

혹시나 직접 찾은 `DDL`로 **Project 구동 시 아래와 같은 에러 문구가 발생**한다면,

**`BATCH_JOB_EXECUTION_PARAMS` Table이 존재하는 지 확인**해보고,

<br>

**`BATCH_JOB_EXECUTION` Table**에 

**`JOB_CONFIGURATION_LOCATION varchar(2500) DEFAULT NULL` Column이 존재하는지 확인** 해주자.

```java
bad SQL grammar [SELECT E.JOB_EXECUTION_ID, E.START_TIME, 
E.END_TIME, E.STATUS, E.EXIT_CODE, E.EXIT_MESSAGE, E.CREATE_TIME, 
E.LAST_UPDATED, E.VERSION, E.JOB_INSTANCE_ID, E.JOB_CONFIGURATION_LOCATION 
from bomdb.BATCH_JOB_EXECUTION E, bomdb.BATCH_JOB_INSTANCE 
I where E.JOB_INSTANCE_ID=I.JOB_INSTANCE_ID and I.JOB_NAME=? 
and E.START_TIME is not NULL and E.END_TIME is NULL order by E.JOB_EXECUTION_ID desc]; 
nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException:
Unknown column 'E.JOB_CONFIGURATION_LOCATION' in 'field list'
```

### JobConfiguration 구현


**여기까지 설정**을 마쳤다면 **이제 Batch Application 관련 Configuration Class를 생성**해

**Batch Job을 생성하여 구동** 해볼 수 있다.

**아래 예제 코드와 같은 Batch Job Configuration Class를 생성**해보자.


```java

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.configuration.annotation.JobBuilderFactory;
import org.springframework.batch.core.configuration.annotation.StepBuilderFactory;
import org.springframework.batch.repeat.RepeatStatus;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.List;

@RequiredArgsConstructor
@Configuration
public class TestJobConfiguration {

    private final JobBuilderFactory jobBuilderFactory;
    private final StepBuilderFactory stepBuilderFactory;
    // 다른 @Service 비즈니스 로직도 DI로 사용 가능
    // private final BatchService batchService;

    @Bean
    public Job testJob() {
        return jobBuilderFactory.get("testJob")
                // testStep1 시작
                .start(testStep1())
                    // testStep1 수행 결과 ExitStatus.FAILED 일 경우
                    .on("FAILED")
                    // failStep1 수행
                    .to(failStep1())
                    // failStep1 수행 결과에 상관 없이
                    .on("*")
                    // testStep1 종료
                    .end()
                // testStep1 후에
                .from(testStep1())
                    // testStep1 수행 결과에 상관 없이
                    .on("*")
                    // tesetStep2로 다음 Step 수행
                    .to(testStep2())
                    // testStep2 수행 결과에 상관 없이
                    // 다음 수행으로 testStep3 지정
                    .next(testStep3())
                    // testStep3 수행 결과에 상관없이
                    .on("*")
                    // stepFlow 종료(FlowBuilder 반환)
                    .end()
                // 해당 Job 종료(FlowBuilder 종료)
                .end()
                .build();
    }
    
    @Bean
    public Step testStep1() {
        return stepBuilderFactory.get("testStep1")
                .tasklet((contribution, chunkContext) ->{
                
                    // Batch 로직 구현
                    ...
                    
                    // 아래 구문으로 로직 도중 FAILED 처리 가능
                    // ExitStatus 안의 값에 따라 분기 가능
                    contribution.setExitStatus(ExitStatus.FAILED);
                    
                    // 정상 종료 시 상태 Return
                    return RepeatStatus.FINISHED;
                })
                .build();
    }
                
    @Bean
    public Step failStep1() {
        return stepBuilderFactory.get("failStep1")
                .tasklet((contribution, chunkContext) ->{
                
                    // Batch 로직 fail 분기 로직
                    ...
                    
                    // 정상 종료 시 상태 Return
                    return RepeatStatus.FINISHED;
                })
                .build();
    }
    
    @Bean
    public Step testStep2() {
        return stepBuilderFactory.get("testStep2")
                .tasklet((contribution, chunkContext) ->{
                
                    // Batch 로직 구현
                    ...
                    
                    // 정상 종료 시 상태 Return
                    return RepeatStatus.FINISHED;
                })
                .build();
    }
        
    @Bean
    public Step testStep3() {
        return stepBuilderFactory.get("testStep3")
                .tasklet((contribution, chunkContext) ->{
                
                    // Batch 로직 구현
                    ...
                    
                    // 정상 종료 시 상태 Return
                    return RepeatStatus.FINISHED;
                })
                .build();
    }
}

```


해당 **`Job` 예제 형식을 참고**하여 **`Job`을 생성**해,

**각 `Step` 마다의 분기로 원하는 `Batch Application`를 구현**해보자.