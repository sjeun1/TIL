## SSH
<br>

### SSH 이란?
SSH(Secure Shell) 란 , 원격지에 있는 컴퓨터를 안전하게 shell 로 제어하는 Protocol 을 뜻한다.<br>
AWS 등의 클라우드 서비스에서도 인스턴스 서버에 접속하려면 일일히 해당 서버가 위치한 곳으로 갈 수 없으니 SSH 를 통해 접속을 하여 서버 작업을 처리한다.<br>
SSH 는 SSH Client 와 SSH Server 로 나눠져있다.<br>
SSH 프로토콜을 통해 Client 가 명령을 내리고 Server 가 명령을 받아 컴퓨터를 제어할 수 있다. <br>
<br>
SSH 클라이언트는 원격 컴퓨터를 연결하는데 사용되는 응용 프로그램인데<br>
리눅스나 macOS 인 경우 OpenSSH 가 있어서 별도 설치가 필요없지만 윈도우 같은 경우에는 별도 설치가 필요하다.<br>
대표적으로 현재 사용하는 putty 나 Xshell 등이 유명한  ssh 클라이언트 프로그램 중 하나이다.<br>
SSH 서버는 SSH 클라이언트를 사용하기 위해 원격으로 연결할 컴퓨터에 설치가 되어있어야한다.<br>
(클라우드 서비스를 사용 중인 경우 SSH Server 설치나 tcp 포트가 방화벽에서 열려있어 별도 설치가 필요없음)<br>
보통 OpenSSH 이 유명한 ssh 서버 종류 중 하나이다. <br>
(원격지 컴에 sudo yum install openssh-server 라는 명령어를 사용하여 설치가 가능하다.<br>
설치 후 which sshd 를 사용해 설치가 됬는지 확인할 수 있다.<br>
ssh 접속을 위해 centos, ubuntu 등의 경우에 따라 iptables, firewalld, ufw 라는 방화벽 프로그램을 설정해야한다.)<br>
<br>

### SSH 사용 이유
이 전에는 Telnet 를 주로 사용했는데 보안적으로 결함이 있다고 한다. Telnet protocol 를 사용했을 때 문자열이 암호화되지 않는다. <br>
이로 인해 중요한 개인 정보가 탈취될 수 있기 때문에 암호화되서 패킷이 보내지는 SSH 를 사용하며,<br>
SSH 는 암호화된 방식으로 정보를 주고 받아 안전하고 보안상에 좋다는 특징을 가지고 있다.<br>
<br>

### SSH 동작원리 
SSH Key 는 public key(공개키), private key(비공개키) 로 이루어지는데 private key 는 접속하는 client 에 public key 는 server 에 위치하게 된다.<br>
SSH 접속을 할 때 두 키를 비교하여 일치 여부를 확인하여 인증하는 방식이다. <br>
(암호화 방식에는 대칭키 방식과 비대칭키 방식이 있는데 대칭키는 동일 키 값으로 암복호화를 하고, 비대칭키는 암복호화를 서로 다른 키로 사용하는 방식이다. )<br>

1. 서버에서 공개키 전달<br>
2. 클라이언트에서 난수 발생 -> 해시값 저장<br>
3. 클라이언트에서 공개키로 암호화 <br>
4. 암호화된 난수값 서버로 전달<br>
5. 비밀키로 복호화->해쉬값 생성<br>
6. 해시값 비교<br>
