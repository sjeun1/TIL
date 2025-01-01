# gitHub Actions로 Docker 배포 자동화

## gitHub Actions
gitHub Actions 는 CI/CD 를 자동화 할 수 있는 github 의 도구이다. <br>
이를 활용하면 docker 이미지를 빌드하고 배포할 수 있으며, 코드 푸시 또는 특정 이벤트 발생 시 자동으로 배포를 실행하도록 설정할 수 있다. <br>
<br>

- workflow
  - github actions 에서 실행되는 자동화 프로세스이다.
  - .github / workflows 디렉터리에 YAML 파일로 정의된다. 
- trigger 
  - workflow 를 실행시키는 조건이다. 
  - ex. push, pull_request, schedule, workflow_dispatch 등.. 
- job
  - workflow 내에서 실행되는 작업 단위이다. 
  - 여러 job 이 병렬 또는 순차적으로 실행 가능하다.
- step
  - job 내에서 실행되는 각 명령어 또는 작업 단위를 말한다. 
- runner
  - workflow를 실행하는 한경이다.
  - github-hosted 또는 self-hosted runner 를 사용할 수 있다.
<br><br>

## docker 
docker 는 애플리케이션과 그 종속성을 컨테이너로 묶어 배포 가능한 환경을 제공한다. <br>
<br><br>
 
## docker 배포 자동화의 흐름
1. docker image build : 코드가 push 되거나 변경될 대 docker 이미지를 생성한다
2. image push : 빌드된 docker 이미지를 docker hub 또는 다른 컨테이너 레지스트리에 업로드한다
3. deploy : 업로드된 이미지를 사용해 컨테이너를 실행한다. (ex. AWS EXS, kubernetes 등.. )
<br><br>

## 실습
### 1) dockerfile 작성
소스코드가 있는 루트 디렉터리에 dockerfile 을 생성하여야한다. <br> 
```
my-application/
├── Dockerfile         # Dockerfile은 루트 디렉토리에 위치
├── package.json       # 애플리케이션의 패키지 파일
├── package-lock.json
├── src/               # 애플리케이션 소스 코드
│   └── index.js
└── .dockerignore      # 빌드 시 제외할 파일을 정의 (선택 사항)
```
- Dockerfile 생성
```
cd {root-directory}
touch Dockerfile
nano Dockerfile
```
- Dockerfile 작성
```
# 1. Java 기반 애플리케이션에 적합한 베이스 이미지 선택
FROM eclipse-temurin:17-jdk

# 2. 작업 디렉토리 설정
WORKDIR /app

# 3. Gradle 빌드에 필요한 파일 복사
COPY build.gradle settings.gradle gradlew gradlew.bat ./
COPY gradle ./gradle

# 4. Gradle 캐시를 활용해 의존성 먼저 다운로드
RUN ./gradlew dependencies --no-daemon

# 5. 전체 소스코드 복사
COPY . .

# 6. 애플리케이션 빌드
RUN ./gradlew build --no-daemon

# 7. 최종 JAR 파일 실행 준비
EXPOSE 8080
CMD ["java", "-jar", "build/libs/your-app.jar"]
```

- Dockerfile 작성 후 검증방법 (빌드 및 실행)
1. 이미지 빌드
```
// 생성될 이미지 이름 지정
// . : Dockerfile 이 있는 현재 디렉터리를 빌드 컨텍스트로 지정한다. 
* 빌드컨텍스트란, 빌드에 필요한 파일들이 저장된 곳을 말한다.

docker build -t {} .

docker build -t dockerimagetest .
```
2. docker 이미지 실행. 컨테이너 실행 
```
docker run -d -p 8081:8081 my-java-app
```
확인
```
http://localhost:8081
```
<br><bt>

### 2) github secrets 설정

### 3) github actions workflow 작성
### 4) docker hub 계정 연결 / 이미지 확인
### 5) 서버에 배포 

