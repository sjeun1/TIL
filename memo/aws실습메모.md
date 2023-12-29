## aws실습내용 (개인메모 기록용)

1. AWS 네트워크 설정<br>
vpc 서브넷<br>
VPC 라는 가상의  네트워크를 하나 만들고 그 안에 서브넷이라는 가상의 VPC 네트워크 환경으로 쪼개는 작업.<br>
<br>

NAT Gateway : 네트워크 주소 변환 서비스 (외부로 나갈때 고정 ip 를 할당, 내부와 별개로)<br>
외부와 외부 통신 - igw (인터넷 게이트웨이 만들고  public 은 라우팅테이블 연결할 때 인터넷 게이트웨이말고 NAT Gateway 로 설정.<br>
<br>
*VPC Dashboard 에서 한번에 설정 가능하다.<br>
<br>

2. AWS Compute <br>
- EC2 인스턴스 보안 그룹 생성<br>
인스턴스 자체적으로는 트래픽 제어 능력X -> 보안그룹 상에서는 나가는 트래픽은 트래픽 ALL, 들어오는 기본적으로 Denied(거부)<br>
보안 그룹 생성할 때 태그 선택 사항 key, value(ex. name, groupName) 하면 비용추적 가능<br>
- key pair 생성<br>
- EC2 인스턴스 생성<br>
스토리지는 외장하드 같은 느낌. EBS <br>
<br>

EC2 - 하이퍼바이저 위에 VMWare <br>
ECS
물리적인 거 신경 안쓰도록<br>
AWS LAMBDA - 인스턴스 신경안쓰고 코드만으로 모든 걸 함. 서버리스<br>
<br>
ami<br>
오토스케일링할때?<br>


<br>
### pem 키 <br />
mkdir ~/.ssh   (복사 하기 전에 디렉토리 만들어야함)<br />
cp ~/Downloads/aws_test.pem ~/.ssh/    (복사) <br />
chmod 600 ~/.ssh/{pem키이름} <br />
mkdir config 해서 config 생성하고 실행까지 해야하므로 chmod 700으로 경로 설정<br />

vim ~/.ssh/config<br />

```
Host {host접속명령어}   (ex. Host a 라고 등록해두면 ssh a 로 해당 EC2 로 접속할 수 있음)
Hostname {탄력적IP주소}
IdentityFile ~/.ssh/{pem키이름}
```





