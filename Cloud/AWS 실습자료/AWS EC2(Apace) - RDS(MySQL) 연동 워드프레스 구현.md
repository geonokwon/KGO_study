# AWS EC2(Apace) - RDS(MySQL) 연동 워드프레스 구현 
1. EC2 대시보드에 EC2 인스턴스 생성
- 이름 : apache
- AMI : Amazon Linux 2
- 인스턴스 유형 : t2.micro
- 키페어(로그인) : 기존의 키 페어 사용
- 네트워크 설정 : 기존 보안 그룹 선택(ssh, web)
- 스토리지 구성 : 8GiB

2. 퍼블릭 IP 주소를 사용하여 원격 접속
- 유저명 : ec2-user

3. EC2 인스턴스 업데이트
sudo yum -y update

4. 웹 서비스 프로그램 apache 설치
sudo yum -y install httpd

5. php 설치
sudo amazon-linux-extras install -y php7.4

6. 아파치 웹 서비스 시작
sudo systemctl start httpd

7. 재시작 시 자동으로 웹 서비스 동작 설정
sudo systemctl enable httpd

8. 동작 확인
sudo systemctl status httpd

9. 첫 시작 페이지 index.html 생성
sudo vi /var/www/html/index.html

10. php 동작 확인
sudo vi /var/www/html/phpinfo.php
=========================
<?php
	phpinfo();
?>

11. 웹 브라우저 주소창에서 확인
EC2 인스턴스 IP주소/index.html
EC2 인스턴스 IP주소/phpinfo.php

---

12. RDS를 MySQL로 구현할 예정이므로 mysql 클라이언트 설치
sudo yum install -y mysql

13.  wordpress 설치
sudo wget https://wordpress.org/latest.tar.gz

14. 압축 해제
sudo tar xvfz latest.tar.gz

15. 압축 해제된 wordpress 폴더를 documentRoot로 복사
sudo cp wordpress /var/www/html

16. wordpress 의 소유자를 apache로 변경
sudo chown -R apache.apache /var/www/html/wordpress

17. RDS 대시보드로 이동, RDS 데이터베이스 설치
---

▪ RDS DB 인스턴스 생성하기
1. AWS 로그인
2. 오른쪽 상단의 리전 확인 (서울 : ap-northeast-2)
3. 콘솔 홈 왼쪽 검색창에 RDS를 검색
4. Amazon RDS 대시보드 왼쪽 항목에서 [데이터베이스] -> [데이터베이스 생성]
5. 데이터베이스 옵션 설정
- 표준 생성
- 엔진 옵션 : MySQL
- 템플릿 : 프리티어
  => 프리티어는 가용성 및 내구성 항목 비활성화, 단일 AZ만 지원
- 식별자 : wordpressDB
- 마스터 사용자 이름 : root
- 마스터 암호 : class10522, 암호확인 : class10522
- 인스턴스 구성 : db.t3.micro
- 스토리지 : 범용 SSD(gp2), 스토리지 용량 : 20 GiB
- ▼ 스토리지 자동 조정 펼침 -> □ 스토리지 자동 조정 활성화 체크 해제!
- 컴퓨팅 리소스 : EC2 컴퓨팅 리소스에 연결 안함
- DB 서브넷 그룹 : default 값 그대로 사용
- 퍼블릭 엑세스 : 예
- 가용 영역 : 기본 설정 없음
- ▼ 추가 구성
  초기 데이터베이스 이름 : wordpress
  □ 자동 백업을 활성화합니다. 체크 해제
  □ 암호화 활성화 체크 해제
  □ 마이너 버전 자동 업그레이드 사용 체크 해제

위의 설정이 완료되면 [데이터베이스 생성] 클릭

18. wordpress의 wp-config.php 생성
cd /var/www/html/wodpress
sudo cp wp-config-sample.php wp-config.php

19. wp-config.php 수정
sudo vi wp-config.php
---
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'root' );
define( 'DB_PASSWORD', 'class10522' );
define( 'DB_HOST', 'itwillbs-rds-class1-team1.cxuw4m6kgp6q.ap-northeast-2.rds.amazonaws.com' );
=> 새로 생성한 Amazon RDS의 엔드포인트를 복사해서 'localhost' 자리에 붙여 넣음!

esc -> :wq

20. 아파치 웹 서비스 재시작
sudo systemctl restart httpd

21. 윈도우 웹 브라우저에서 EC2 인스턴스 IP주소/wordpress 를 입력하면 워드프레스의 첫 설치 과정으로 넘어감!


