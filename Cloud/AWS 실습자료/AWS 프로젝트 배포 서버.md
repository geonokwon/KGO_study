# AWS 프로젝트 배포 서버 
- 톰캣 : 10.1.31
- JDK : 17

1. EC2 인스턴스 생성
- 이름 : ApacheTomcat
- AMI : Amazon Linux 2
- 인스턴스 유형 : t2.micro
- 키페어(로그인) : 기존 것 사용 가능
- 보안그룹 : ssh, web
- 스토리지 구성 : 기본값(8GiB)

2. 원격 접속

3. 패지키 업데이트
sudo yum update -y

4. JDK 17 설치
# Amazon Corretto 17 레포지토리 추가
sudo rpm --import https://yum.corretto.aws/corretto.key
sudo curl -L -o /etc/yum.repos.d/corretto.repo https://yum.corretto.aws/corretto.repo

# JDK 17 설치
sudo yum install -y java-17-amazon-corretto-devel

# 설치 확인
java -version

5. Tomcat 10 설치
# Tomcat 10 다운로드
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.31/bin/apache-tomcat-10.1.31.tar.gz

# 압축 파일
sudo tar xvfz apache-tomcat-10.1.31.tar.gz

# 작업 디렉터리로 옮기기
sudo mv apache-tomcat-10.1.31 /usr/local/tomcat

# 불필요한 압축파일 삭제(선택)
sudo rm apache-tomcat-10.1.31.tar.gz

6. 환경 변수 설정
sudo vi /etc/profile.d/tomcat.sh

# 내용 추가
export CATALINA_HOME=/usr/local/tomcat
export PATH=$PATH:$CATALINA_HOME/bin

# 환경변수 적용
source /etc/profile.d/tomcat.sh


7. Tomcat 권한 설정
# 실행 권한 부여
sudo chmod +x /usr/local/tomcat/bin/*.sh 
※ 위의 명령어 실행 시 에러가 발생하면 root 유저로 변경 후 다시 입력
sudo -sE -> root 유저 변경 후 위의 명령어 입력 -> 입력이 끝나면 exit

# 소유자 변경(선택사항)
sudo chown -R ec2-user:ec2-user /usr/local/tomcat

8. Tomcat 시작
# Tomcat 시작
$CATALINA_HOME/bin/startup.sh

# 상태 확인
ps -ef | grep tomcat

9. 시스템 부팅 시 자동 시작 설정(선택사항)
sudo vi /etc/systemd/system/tomcat10.service

# 아래 내용 추가
[Unit]
Description=Apache Tomcat 10
After=network.target

[Service]
Type=forking
Environment=JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto
Environment=CATALINA_HOME=/usr/local/tomcat
Environment=CATALINA_BASE=/usr/local/tomcat

ExecStart=/usr/local/tomcat/bin/startup.sh
ExecStop=/usr/local/tomcat/bin/shutdown.sh

User=ec2-user
Group=ec2-user
RestartSec=20
Restart=always

[Install]
WantedBy=multi-user.target

# 서비스 등록 및 시작
sudo systemctl daemon-reload
sudo systemctl start tomcat10
sudo systemctl enable tomcat10

# 서비스 확인
sudo systemctl status tomcat10

10. 설치 확인
# Tomcat 웹 페이지 접속 확인
http://EC2-퍼블릭-IP:8080



