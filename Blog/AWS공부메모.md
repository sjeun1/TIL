IaaS : 인프라만 제공 (computing, storage, network )
가상의 컴퓨터만 제공
ex) AWS EC2

PaaS : 인프라+OS+기타 프로그램 실행에 필요한 부분
코드만 올려서 돌릴 수 있음
ex) Firebase, Google App Engine

SaaS : 서비스 자체 제공
ex)Gmail, Slack, Google Docs
<br><br><br>


-&nbsp;AWS 구분
크게 글로벌 서비스, 리전으로 구분됨
글로벌 서비스 : IAM, Amazon ClouldFront, Route53
리전 & 가용 X : VPC, S3
리전 & 가용영역 : RDS, EC2
<br><br><br>


-&nbsp;리전 선택 시 고려할 점 
지연 속도, 법률, 사용 가능한 AWS 서비스 (리전마다 사용 가능 서비스가 다름)
<br>

가용영역이란 하나이상의 데이터 센터
일반적으로 반드시 1리전, 2개 이상의 가용 영역이 있음
반드시 물리적으로 일정 거리 떨어져 있어야 함 (자연재해 등 방지를 위해)
<br><br><br>


-&nbsp;엣지 로케이션 
AWS 의 CloudFront (CDN) (임시저장공간) 등의 여러 서비스 들을 가장 빠른 속도로 제공(캐싱)하는 거점
<br><br><br>


-&nbsp;Amazon Resource Name (ARN)
AWS 의 모든 리소스의 고유 아이디
<br><br><br>


-&nbsp;AWS 유저
크게 루트 유저와 IAM 유저로 나뉜다.

루트 유저 
탈취 당했을 때 복구가 힘드므로 사용 자제 및 MFA 설정 필요
루트 유저는 관리용으로만 이용 : 계정 설정 변경, 빌링 등

IAM 유저
Identity and Access Management (IAM) 을 통해 생성한 유저
관리자 IAM User, 개발자 IAM USer, 감사팀 IAM User 등 각각 따로 권한 부여해야함
권한 부여 시 루트 유저와 같이 모든 권한 가질 수 있음, 빌링 관련 권한은 루트 유저의 허용이 필요함
<br><br><br>


-&nbsp;EC2 (Elastic Compute Cloud) : aws 에서 제공하는 성능, 용량 등을 유동적으로 사용할 수 있는 서버이다.<br>

-&nbsp;region : aws 의 서비스가 구동될 지역을 이야기하는데 국내 서비스라면 무조건 서울 리전을 선택하면 된다. <br>
-&nbsp;인스턴스 : EC2 서비스에 생성된 가상머신 <br>
AMI (Amazon Machine Image) : EC2 인스턴스를 시작하는데에 있어 필요한 정보를 이미지로 만들어 둔 것이다.<br>
-> 필요한 소프트웨어 구성(운영체제, 애플리케이션 서버 및 애플리케이션)이 포함된 템플릿<br>
-&nbsp;스토리지 : 서버의 디스크 (하드디스크, SSD 등)를 뜻하며 서버의 용량을 설정.<br>
-&nbsp;보안그룹 : 방화벽 (0.0.0.0/0, ::/0 같은 전체 오픈을 하는 경우, 깃허브 등에 실수로 pem 키가 노출되는 순간 채굴로 쓰임…)<br>

-&nbsp;EIP(Elastic IP) : 요금을 아끼기위해 인스턴스 중지하고 다시 시작하면 매번 IP가 변경되므로 고정 IP 를 설정한다. <br>
사용중일 경우 비용이 발생하지 않지만, 그렇지 않으면 비용이 부과되기 때문에 EC2 에 바로 연결하거나 미사용 EIP는 삭제해줘야한다. <br>
<br>
