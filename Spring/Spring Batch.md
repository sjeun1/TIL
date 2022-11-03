
# spring batch 
<br>

## spring batch 특징
1. 로깅추적, 트랜잭션 관리, 작업 처리 통계, 리소스 관리 등 최적화 및 파티셔닝 기술을 통해 대용량 처리 가능하다.<br>
2. 중복 실행 방지 기능이 있다.<br>
3. 배치 실행하다가 중간에 실패할 경우 실패한 시점부터 다시 실행된다.<br>
4. scheduler 로서 동작 기능이 없어 외부 스케쥴러에 의해 실행된다. 그 자체로는 주기적, 반복적 수행을 할 수 없다<br>
<br>

## 배치 핵심패턴
-&nbsp;Read: 데이터베이스, 파일, 큐에서 다량의 데이터를 조회한다<br>
-&nbsp;Process: 특정 방법으로 데이터를 가공한다<br>
-&nbsp;Write: 데이터를 수정된 양식으로 다시 저장한다<br>
<br>

## 배치 시나리오
프로세스를 주기적으로 커밋 (한번에 하면 메모리에 부담)<br>
동시다발적인 Job 의 배치처리, 대용량인 경우 병렬 처리. 멀티쓰레드로 Job을 처리<br>
실패 후 수동 또는 스케줄링에 의한 재시작<br>
의존관계가 있는 step 여러개를 순차적 처리. (경우에 따라서는 step 순서대로 X. flow 에 따라 유연하게 구성)<br>
반복, 재시도, Skip 처리<br>
<br>

## 아키텍처
총 3개의 layer 로 구성 되어 있다<br>
application : 개발자가 만든 모든 배치 Job<br>
batch core : Job을 실행, 모니터링, 관리하는 API 로 구성. JobLauncher, Job, Step, Flow 등이 속함<br>
batch Infrastructure : application, Core 모두 공통 Infrastructure 위에서 빌드. Job 실행의 흐름과 처리를 위한 틀 제공. Reader, Processor Writer, Skip, Retry 등이 속함<br>
<br>

배치 초기화 설정 클래스는 순서대로<br>
@EnableBatchProcessing<br>
SimpleBatchConfiguration<br>
BatchConfigurerConfiguration ( BasicBatchConfigurer, JpaBatchConfigurer )<br>
BatchAutoConfiguration<br>
<br>

jobBuilderFactory 로 job 객체 생성<br>
stepBuilderFactory 로 Step 객체 생성<br>
tasklet으로 tacklet 객체 생성<br>
각각 구동되면 다름 단계가 실행되도록 설정함<br>
job 은 일, 일감. step 은 일의 항목, 단계. tasklet 은 작업 내용. 비지니스로직들<br>
<br>

## DB Schema
![image](https://user-images.githubusercontent.com/62210870/197560912-bb2bd596-2937-4bef-83ce-ccf5c84e2d42.png)
<br>
BATCH_JOB_INSTANCE : Job 실행될 때 JobInstance 정보 저장, job_name과 job_key를 키로 하여 데이터가 저장. 동일한 job_name 과 job_key 로 중복 저장 X<br>
BATCH_JOB_EXECUTION : Job 의 실행 정보가 저장. Job 생성, 시작, 종료시간, 실행 상태, 메시지 등을 관리<br>
BATCH_JOB_EXECUTION_PARAMS : Job 과 함께 JobParameter 정보를 저장<br>
BATCH_JOB_EXECUTION_CONTEXT : Job 실행동안 여러가지 상태정보, 공유 데이터를 직렬화(Json 형식) 해서 저장. Step 간 서로 공유 가능함.<br>
BATCH_STEP_EXECUTION : Step 의 실행정보가 저장. 생성, 시작, 종료시간, 실행상태, 메시지 등을 관리<br>
BATCH_STEP_EXECUTION_CONTEXT : Step 의 실행동안 여러가지 상태정보, 공유 데이터를 직렬화(Json 형식) 해서 저장. Step 별로 저장되며 Step 간 서로 공유 불가능<br>
<br>

## 배치 도메인 이해
1. Job<br>
  1. Job <br>
  2. JobInstance<br>
  3. JobParameters<br>
  4. JobExecution<br>
2. Step<br>
3. ExecutionContext<br>
4. JobRepository / JobLauncher<br>
<br>

#### Job <br>
1. Batch Layer Struct 에서 가장 상위에 있는 개념. 하나의 배치 작업 자체를 의미한다.<br>
2. Job Configuration 을 통해 생성되는 객체 단위로서 배치 작업을 어떻게 구성하고 실행할 것인지 전체적으로 설정하고 명세해둔 객체를 의미한다. <br>
3. 최상위 인터페이스이며, 여러 Step 을 포함하고 있는 컨테이너로서 반드시 한개 이상의 Step 으로 구성해야 한다.<br>
4. 순차적으로 Step 을 실행시키는 Job 인 SimpleJob 과 특정한 조건이나 흐름에 따라 Step 을 실행시키는 FlowJob 이 있다


<br><br><br>

출처 및 참고<br>
인프런 정수원님 강의: 스프링 배치 - Spring Boot 기반으로 개발하는 Spring Batch ( https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B0%B0%EC%B9%98/dashboard ) <br>
스프링 문서 ( https://spring.io/projects/spring-batch#overview ) <br>
