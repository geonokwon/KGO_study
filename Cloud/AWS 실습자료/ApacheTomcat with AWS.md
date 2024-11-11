#  ApacheTomcat with AWS 
1. 인스턴스 생성
EC2 대시보드 -> 왼쪽 메뉴 인스턴스 -> 인스턴스 이동 -> 인스턴스 시작
- 이름 : ApacheTomcat
- AMI : Amazon Linux 2
- 인스턴스 유형 : t2.micro
- 키페어 : 기존의 키페어 사용
- 네트워크 : 기존 보안 그룹 선택 -> ssh, web
- 스토리지 구성 : 기본값 8GiB
설정이 완료되면 인스턴스 시작 -> 모든 인스턴스 보기

2. AWS 방화벽 설정
- AWS 방화벽은 웹에서 설정
EC2 대시보드 -> 네트워크 및 보안 -> 보안그룹 -> web 선택 -> 아래쪽의 인바운드 규칙탭 클릭 -> 인바운드 규칙 편집
=> 유형 : 사용자 지정 TCP, 프로토콜 : TCP(변경불가), 포트 범위 : 8080
=> 소스 유형 : Anywhere-Ipv4 선택
규칙 추가가 끝나면 오른쪽 아래의 [규칙 저장] 버튼 클릭

3. 원격 접속
- 설치되어 있는 원격 접속 프로그램 사용(MobaXterm)
- Session -> New Session -> SSH
  Remote host : EC2 인스턴스의 퍼블릭 IPv4 주소
  Advanced SSH settings 탭 이동 -> □Use private key 선택 -> 생성된 키페어 파일을 클릭
  Bookmark settings 탭 -> session name : ApacheTomcat
- 설정이 끝나면 OK 버튼 클릭
- 팝업창이 뜨면 'Accept' 선택
- login as : ec2-user
=> 원격 접속 완료!!

4. EC2 인스턴스 업데이트
sudo yum update -y

5. 웹 서버로 동작
- 웹 서비스 프로그램 필요
  (웹 서비스 프로그램 : Apache, Nginx, IIS 등)
- Apache Web Service 사용

sudo yum install -y httpd
=> 아파치 웹 서비스 프로그램(패키지) 설치

sudo systemctl status httpd
=> 아파치 웹 서비스 상태 확인
=> 중지 중 - Active: inactive(dead)

sudo systemctl start httpd
=> 아파치 웹 서비스 시작

※ Apache(httpd)
- 설정 파일 : /etc/httpd/conf/httpd.conf
- 도큐먼트 루트 : /var/www/html

6. 웹 서비스 동작 확인
- 보안 그룹 인바운드 규칙 중 HTTP(TCP 80)가 허용 되어 있어야 함
- 윈도우 웹 브라우저에서 EC2 인스턴스의 퍼블릭 IPv4 주소를 주소창에 입력
=> Test Page가 보임
=> /var/www/html/ 디렉터리 내에 index.html이 없으므로 에러 페이즈를 보여주지 않고 미리 약속된 아파치의 Test Page가 보이는 것

7. 웹 접속 시 보여줄 시작 페이지(index.html) 만들기
sudo vi /var/www/html/index.html
=> 'i' 눌러서 입력모드 전환 -> 간단한 내용 입력
=> esc -> :wq 저장 

8. 톰캣 서비스 파일 다운로드
- 기본적으로 html 관련 처리는 아파치가 하고, java와 관련된 처리는 톰캣이 하게 됨
- 톰캣 설치 파일을 인터넷을 통해 다운로드

웹 브라우저 tomcat.apache.org 사이트 접속
왼쪽 항목 Download -> Tomcat9 선택 -> 스크롤을 내려서 Core -> tar.gz 항목 우클릭 -> 링크 복사 선택

다시 원격 접속 창으로 이동

curl -O URL 주소 명령어를 입력해서 인터넷을 통해 Tocmat9 버전을 다운로드 받음
※ wget URL 주소 명령어도 사용 가능

curl -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz
=> 현재 위치한 디렉터리(폴더)에 파일이 다운로드 됨
=> ls 명령어를 통해서 확인 가능
=> 빨간색으로 보이는 파일은 리눅스 환경에서 압축 파일

9. 톰캣 설치 파일 압축 해제
tar xvfz apache-tomcat-9.0.91.tar.gz
=> apache-tomcat-9.0.91 이름의 디렉터리(폴더)가 생겨남

10. 압축 해제된 파일 위치 이동
- cp(copy) 또는 mv(move) 명령어를 활용
  cp 원본파일 복사될 위치(~을 ~로)
  mv 원본파일 이동할 위치(~을 ~로)
=> cp는 원본파일이 그대로 유지, mv는 원본파일이 해당 디렉터리에서 삭제

sudo cp -r apache-tomcat-9.0.91 /usr/local/tomcat
=> apache-tomcat-9.0.91 폴더를 /usr 아래의 local 디렉터리 내에 tomcat 이라는 이름으로 바꿔서 복사하겠다! 

ls
=> cp 명령어를 사용했으므로 apache-tomcat-9.0.91 디렉터리가 여전히 존재

ls /usr/local
=> tomcat 이라는 이름의 디렉터리(폴더)가 보임!

11. OpenJDK11 설치
yum list java*
=> java 시작하는 패키지(프로그램) 리스트가 보임
=> OpenJDK11 이 보이지 않음

amazon-linux-extras
=> AWS에서 제공하는 외부 저장소의 패키지를 볼 수 있는 명령어
=> 33번 항목에서 java-openjdk11 을 찾을 수 있음

sudo amazon-linux-extras install java-openjdk11 -y
=> openjdk11 버전 설치

java -version 명령어로 설치 확인(버전 확인)

javac -version
=> javac 미활성화

sudo yum install java-11-openjdk-devel -y
=> javac 활성화

javac -version
=> javac 버전이 보이면 성공

12. 톰캣 서버 시작 / 중지
sudo sh /usr/local/tomcat/bin/startup.sh
=> 톰캣 시작

sudo sh /usr/local/tomcat/bin/shutdown.sh
=> 톰캣 중지

13. PATH 설정
readlink -f /usr/bin/java
=> 경로 확인
=> usr/lib/jvm/java-11-openjdk-11.0.23.0.9-2.amzn2.0.1.x86_64
  
sudo vi /etc/profile
- profile 파일은 윈도우의 '시작 프로그램'을 생각하면 됨
- 리눅스가 처음 시작될 때 profile 내의 내용을 먼저 시작
- 가장 아래쪽 새 라인에 입력
=========================
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.23.0.9-2.amzn2.0.1.x86_64
CATALINA_HOME=/usr/local/tomcat
PATH=$PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin
CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar
export JAVA_HOME CATALINA_HOME PATH CLASSPATH
\=============================
esc -> :wq

source /etc/profile

14. Tomcat 서비스를 systemd 에서 제어
- 패키지(프로그램)를 yum 명령어로 설치하는 경우 systemd로 제어가 가능
- systemctl 명령어로 프로그램을 켜고, 끄고, 재시작을할 수 있음
- yum 명령어가 아닌 수동으로 패키지를 설치했다면 systemctl 명령어로 제어를할 수 없음
- 따라서 수동으로 설치한 패키지를 systemctl 명령어로 제어하기 위해서는 별도의 과정이 필요

sudo vi /etc/systemd/system/tomcat9.service

※ service 파일음 1)Unit 2)Service 3)Install 3가지 항목으로 구성
\========================
Description=Apache Tomcat9
After=syslog.target network.target

## Service
Type=forking
ExecStart=/usr/local/tomcat/bin/startup.sh
ExecStop=/usr/local/tomcat/bin/shutdown.sh

User=root
Group=root

Umask=0007
RestartSec=10
Restart=always

## Install
WantedBy=multi-user.target
\=======================
esc -> :wq

sudo systemctl daemon-reload
=> 변경된 내용을 적용

15. systemctl 명령어로 톰캣 서비스 시작/중지
sudo systemctl start tomcat9
sudo systemctl stop tomcat9

source /etc/profile
=> 재부팅 없이 profile에 수정된 내용 적용

#  AWS EC2 인스턴스 파일 업로드 
- 기본적으로 윈도우 운영체제와 리눅스 운영체제는 파일 교환이 불가능!!
- 파일을 주고 받기 위해서는 파일 서비스가 필요
- 리눅스를 파일 서버로 설정하고 윈도우에서 클라이언트 프로그램을 설치해야 파일을 업로드할 수 있음.
- 서버- 클라이언트 환경을 만들지 않아도 쉽게 파일 전송을할 수 있는 방법이 있는데 바로 SFTP를 사용하는 방법

/usr/local/tomcat/webapps에 바로 파일을 업로드 하려고 하면 에러가 발생!!
=> root 소유자 이기 때문

해결방법: ec2-user 유저의 홈 디렉터리인 /home/ec2-user 디렉터리에 우선 파일을 업로드 하고, 명령어를 통해 해당 위치로 이동
1) 원하는 파일을 /home/ec2-user 방에 업로드(Drag&Drop)
2) 터미널 창에서 cd 엔터를 눌러서 ec2-user의 홈 디렉터리 이동
3) ls 명령어를 통해서 파일 확인
4) sudo mv Project명.war /usr/local/tomcat/webapps/
=> 자동으로 압축해제

웹 브라우저에서 서버IP:8080/프로젝트명/파일명 으로 접속!
   

