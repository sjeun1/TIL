## docker (docker-compose 사용과 nginx 컨테이너 실행)
<br>

## Nginx 와 Docker Compose 
Nginx 는 정적 파일 제공, 리버스 프록시, 로드 밸런싱 등에 사용되는 웹 서버 소프트웨어이다. <br>

Docker Compose 는 멀티 컨테이너 애플리케이션을 정의하고 실행하기 위한 도구이며 <br>
여러 컨테이너의 설정을 하나의 파일 (docker-compose.yml) 로 정의하여 관리가 가능하다. <br>
Docker Compose 를 사용하여 Nginx 와 관련된 설정(볼륨과 네트워크 설정, Nginx-backend 연결 등.. ) 을 간단하게 관리 할 수 있기 때문에 함께 사용하였다. <br>

Nginx 참고 : https://www.papertrail.com/solution/guides/nginx/ <br>
Docker Compose : https://docs.docker.com/compose/ <br>
<br>
<br>

## nginx 컨테이너 실행
#### 새로운 컨테이너 실행 
```
docker run --name nginx-container -d -p 8081:80 nginx
```

docker: Error response from daemon: Conflict. The container name "/nginx-container" is already in use by container <br>
오류로 실행 중인 컨테이너를 중지하였고, 사용하지 않은 컨테이너였기 때문에 삭제 처리하였다. <br>
만약 필요하다면 기존 컨테이너 상태 확인 후 이름 사용해도 됨 <br>

#### 컨테이너 상태 확인
```
docker ps -a
```

#### 실행중인 컨테이너 중지
```
docker stop nginx-container
```

#### 컨테이너 삭제
```
docker rm nginx-container
```
<br><br>


## nginx 설정 파일(nginx.conf) 작성
```
mkdir nginx-docker
cd nginx-docker
```

```
touch nginx.conf
```

```
mkdir html
echo "<h1>Welcome to NGINX!</h1>" > html/index.html
```









## Docker Compose 로 nginx 와 백엔드 컨테이너 정의

### docker-compose.yml 생성 
```
mkdir nginx-docker
cd nginx-docker
nano docker-compose.yml (또는 vim 으로 생성)
```

#### docker-compose.yml 작성
```
version: '3.9'

services:
  nginx:
    image: nginx:latest // 최신버전 호출 
    container_name: nginx-container
    ports:
      - "38080:80"
    volumes: 
      - ./html:/usr/share/nginx/html
    restart: always
```
volumes 은 컨테이너와 호스트 시스템 간에 디렉토리 또는 파일을 공유하기 위해 연결하는 것 으로, <br>
컨테이너 내부파일을 호스트에서 바로 읽거나 수정이 가능하고, 컨테이너 재생성 시에도 데이터가 유지될 수 있다. <br>


<br><br>















