IaaS : 인프라만 제공 (computing, storage, network )
가상의 컴퓨터만 제공
ex) AWS EC2

PaaS : 인프라+OS+기타 프로그램 실행에 필요한 부분
코드만 올려서 돌릴 수 있음
ex) Firebase, Google App Engine

SaaS : 서비스 자체 제공
ex)Gmail, Slack, Google Docs



- AWS 구분
크게 글로벌 서비스, 리전으로 구분됨
글로벌 서비스 : IAM, Amazon ClouldFront, Route53
리전 & 가용 X : VPC, S3
리전 & 가용영역 : RDS, EC2




- 리전 선택 시 고려할 점 
지연 속도, 법률, 사용 가능한 AWS 서비스 (리전마다 사용 가능 서비스가 다름)

가용영역이란 하나이상의 데이터 센터
일반적으로 반드시 1리전, 2개 이상의 가용 영역이 있음
반드시 물리적으로 일정 거리 떨어져 있어야 함 (자연재해 등 방지를 위해)




- 엣지 로케이션 
AWS 의 CloudFront (CDN) (임시저장공간) 등의 여러 서비스 들을 가장 빠른 속도로 제공(캐싱)하는 거점




- Amazon Resource Name (ARN)
AWS 의 모든 리소스의 고유 아이디



- AWS 유저
크게 루트 유저와 IAM 유저로 나뉜다.

루트 유저 
탈취 당했을 때 복구가 힘드므로 사용 자제 및 MFA 설정 필요
루트 유저는 관리용으로만 이용 : 계정 설정 변경, 빌링 등

IAM 유저
Identity and Access Management (IAM) 을 통해 생성한 유저
관리자 IAM User, 개발자 IAM USer, 감사팀 IAM User 등 각각 따로 권한 부여해야함
권한 부여 시 루트 유저와 같이 모든 권한 가질 수 있음, 빌링 관련 권한은 루트 유저의 허용이 필요함




- 
