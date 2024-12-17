## Kubernetes - Minikube 설치 및 배포

### 기본개념
kubernates 는 컨테이너화된 애플리케이션 서비스를 관리하기 위한 오픈 소스 플랫폼이다. <br>
자동화, 스케일링, 복구하고 클러스터 환경에서 안정적이고 효율적인 배포 및 관리를 가능하게 한다. <br>
<br>

Pod : 가장 작은 배포 단위로, 컨테이너가 실행되는 논리적 호스트(가상 실행 환경)이다. <br>
Node : Node는 클러스터의 구성 요소이며, Pod가 실행되는 물리적 또는 가상 머신이다. <br>
Cluster : 클러스터는 애플리케이션을 실행하고 트래픽을 분배하며, 자동 복구와 같은 작업을 지원하며, 여러 Node가 모여 하나의 Cluster 를 구성한다. <br>
Deployment : Pod 와 관련된 배포 전략을 정의하며, 애플리케이션을 쉽게 배포, 업데이트, 롤백할 수 있다. (장애 발생한 Pod 재생성, 점진적 배포 등) <br>
Service : Pod 에 대해 안정적인 네트워크 접점을 제공하며, load banlancing 과서비스 검색 기능을 통해 클라이언트는 Pod 변경을 알필요 없이 연결이 가능하다. <br>
ConfigMap : 데이터베이스 연결 문자열, API Key 외부화 등의 환경 설정을 저장하고 관리하는 객체이다. <br>
<br>
<br>

### 클러스터와 Minikube
클러스터는 여러 대의 컴퓨터(노드)를 하나의 시스템처럼 묶어서 운영하는 것으로 Kubernetes 에서 컨테이너를 배포하고 관리한다. <br>

Minikube는 로컬 환경에서 Kubernetes 클러스터를 빠르고 쉽게 실행하기 위한 도구이다. <br>
Minikube 를 사용함으로서 로컬환경에서 Kubernetes 실습하며, 컨테이너화하고 Kubernetes 배포 설정을 검증할 수 있다. <br>
클라우드 Kubernetes 클러스터는 비용이 발생하지만, Minikube는 무료로 로컬에서 실행 가능하다.  <br>
<br><br>

### 왜 Docker Compose 대신 Kubernetes 를 사용하는지,,?
Docker Compose 는 작은 규모의 애플리케이션에서 사용하기 용이하며, 스케일링, 내장복구, 클러스터 관리 기능이 없다. <br>
하지만 kubernetes 는 자동 스케일링을 지원하고, Pod 의 장애를 자동으로 감지하여 Node 실패 시 다른 Node 로 Pod 를 재배치할 수 있다. <br>
그리고 분산 환경에서 수백 대 이상의 Node 를 관리할 수 있는 기능(자동 배포, 롤백, 로드밸런싱)이 존재한다. <br>
<br><br>

## 환경설정 
### docker와 kubectl 확인
```
docker --version
kubectl version --client
```

### Minikube 설치 (Apple M1/M2 칩셋(ARM 아키텍처)을 사용하는 경우 ARM64 버전을 설치해야함!)
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64
sudo install minikube-darwin-arm64 /usr/local/bin/minikube
```
![image](https://github.com/user-attachments/assets/b4992c41-31e5-4936-b505-60d09ac25455)

## Minikube 로 클러스터 실행
```
minikube start
minikube status
```
## 배포
### Kubernetes Deployment 작성, 간단한 Nginx 웹 서버 배포
Kubernetes Deployment 작성
```
kubectl create deployment nginx-deployment --image=nginx
```

애플리케이션에 접근할 수 있도록 NodePort로 노출
```
/*
컨테이너 내부 포트이며, Nginx Docker 이미지의 기본 포트는 80이, 컨테이너 내부에서 웹 서버가 실행되는 포트를 뜻함
NodePort 를 사용하여 외부에서 접근할 수 있도록 함
일반적으로 NodePort는 30000~32767 범위
*/

kubectl expose deployment nginx-deployment --type=NodePort --port=30000

// 포트 변경으로 삭제 했다가 재생성함
kubectl delete service nginx-deployment
```

pod 상태 확인, 서비스 확인
```
kubectl get pods

kubectl get services
```
Minikube가 할당한 IP와 포트를 확인
```
minikube service nginx-deployment --url
```

## Minikube 종료
```
minikube stop
minikube delete
```


