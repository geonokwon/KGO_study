#  AWS EC2 WordPress 설정 
- 워드프레스는 웹사이트를 간단히 만들 수 있는 프로그램
- 웹 서비스 프로그램, PHP, 데이터베이스 필요
- 웹 서비스 프로그램 : Apache
  데이터베이스 : MariaDB

1. EC2 인스턴스 생성
- 인스턴스 대시보드 -> 인스턴스 -> 인스턴스 -> 인스턴스 시작 버튼 클릭
- 이름 : wordpress
- AMI : Amazon Linux 2 AMI(HVM)
- 인스턴스 유형 : t2.micro
- 키 페어(로그인) : 기존이 것 사용(class1_key)
- 네트워크 설정 : 기존 보안 그룹 선택(ssh, web)
- 스토리지 구성 : 8GiB gp2(기본값)
=> 인스턴스 시작 => 모든 인스턴스 보기

2. 원격접속
- MobaXterm 활용
- Session -> SSH -> Remote Host : EC2 퍼블릭 IP주소 입력
- Advanced tap에 가서 키 등록
=> 접속 -> Accept -> login as : ec2-user

3. 워드프레스에 필요한 요소가 있는지 확인
rpm -qa httpd php mariadb-server

4. 패키지(프로그램) 설치
sudo yum -y update && sudo yum -y install httpd php php-mysqlnd mariadb-server

5. 다시 필요 요소 설치 확인
rpm -qa httpd php mariadb-server
=> php-5.4.16-46.amzn2.0.5.x86_64
     httpd-2.4.61-1.amzn2.0.1.x86_64 
     mariadb-server-5.5.68-1.amzn2.0.1.x86_64

6. 아파치 웹 서비스 동작 확인
sudo systemctl status httpd
=> Active: inactive(dead)

7. 아파치 웹 서비스 시작
sudo systemctl start httpd

8. 아파치 웹 서비스 동작 확인
sudo systemctl status httpd
=>  Active: active (running)

9. 윈도우 웹 페이지에서 EC2 인스턴스 IP주소를 주소창에 입력하면 Test Page가 뜸!

10. 간단한 웹 페이지 작성
DocumentRoot : /var/www/html
sudo nano /var/www/html/index.html
=> 내용 입력
=> Ctrl + x -> y -> 엔터
※ ^(= 키보드 자판의 Ctrl 키와 동일)

11. 다시 웹 브라우저 이동해서 새로고침(F5)을 하면 입력한 내용이 화면에 보임!

12. PHP가 잘 동작하는지 확인
- PHP로 작성된 파일이 /var/www/html 아래에 있다면 확인 가능
sudo vi /var/www/html/phpinfo.php
---
<?php
    phpinfo();
?>
---
esc -> :wq

13. php 페이지를 적용하기 위해서 아파치 웹 서비스 다시 시작
sudo systemctl restart httpd

14. 웹 브라우저로 이동하여 EC2인스턴스 IP주소/phpinfo.php 주소창에 입력
=> 웹 브라우저 화면에 php 관련 페이지가 표시

15. 데이터베이스 시작 및 접속
- 데이터베이스는 주로 MySQL 또는 MariaDB를 사용

sudo systemctl start mariadb
=> 마리아 DB 서비스 시작

mysql -uroot (엔터)

show databases;
=> 데이터베이스(스키마) 확인

create database wpDB;
=> 워드프레스가 사용할 wpDB 데이터베이스 생성

show databases;
=> 생성된 wpDB 데이터베이스 확인

16. wordpress가 데이터베이스와 연동될 때 사용할 사용자 생성
(grant 권한 on 대상 to 사용자 identified by 비밀번호)
grant all on wpDB.* to wpUser@localhost identified by '1234';

flush privileges;
=> 사용자 변경에 대한 설정 적용

exit

17. 워드프레스 프로그램 설치
wget https://ko.wordpress.org/wordpress-4.9.6-ko_KR.tar.gz

ls
=> wordpress-4.9.6-ko_KR.tar.gz 압축 파일 확인

tar xvfz wordpress-4.9.6-ko_KR.tar.gz
=> 압축 해제
=> 현재 디렉터리에 'wordpress' 디렉터리(폴더)가 생성됨

18. wordpress 디렉터리를 아파치의 Document로 이동
sudo mv wordpress /var/www/html 

19. 소유자, 퍼미션(허가권) 변경
- 현재 wordpress 디렉터리의 소유자는 ec2-user
=> ec2-user로 wordpress를 설치했기 때문!
- wordpress를 실제로 사용하는 유저는 apache 여야 함.

chown : change owner
chown [소유자.소유그룹] [대상]

sudo chown -R apache.apache /var/www/html/wordpress
※ -R : wordpress 아래의 모든 디렉터리와 파일에 대한 권한을 한꺼번에 변경

ls -l /var/www/html
=> drwxr-xr-x 5 apache apache 4096 Jun 10  2018 wordpress
=> 소유자가 ec2-user에서 apache로 변경됨

chmod : change modify
sudo chmod 777 /var/www/html/wordpress
현재 허가권 : rwxr-xr-x(소유자는 읽고,쓰고,실행 가능 | 소유그룹 읽고, 실행 | 그 밖의 사람 읽고, 실행)
변경될 허가권 : rwxrwxrwx
=> 권한 변경
=> 소유자/소유그룹/그밖에 유저 모두에게 읽고, 쓰고, 실행하는 권한을 부여

20. wordpress 디렉터리로 이동, 설정 파일 생성
cd /var/www/html/wordpress

- 워드프레스 설정을 하기 위해서는 wp-config.php 파일이 필요
- 해당 파일을 생성하는데 wp-config-sample.php  파일을 이용
cp wp-config-sample.php wp-config.php
=> 샘플 파일을 복사

vi wp-config.php
:set nu

23번 라인
database_name_here -> wpDB 로 변경

26번 라인
username_here -> wpUser 로 변경

29번 라인
password_here -> 1234로 변경

---

변경이 끝나면
esc -> :wq

21. 윈도우 웹 브라우저 이동 EC2인스턴스 IP주소/wordpress 입력
=> 환경합니다!

사이트 제목 : 아이티윌 부산교육센터
사용자명 : admin
비밀번호 : admin@1234
※ 비밀번호의 경우 약한 비밀번호를 입력하면 아래에 비밀번호 확인 항목이 생겨남, 체크

정보가 입력되었으면 아래쪽의 [워드프레스 설치하기] 클릭

로그인 화면이 나오면 위에서 입력한 ID/PW 입력
아이디 : admin
비밀번호 : admin@1234

웹 브라우저를 하나 더 실행해서 EC2 IP주소/wordpress 를 주소창에 입력하면 wordpress 화면이 보임!

22. EC2 IP주소만 입력해도 wordpress 화면을 보고 싶다면 httpd(아파치) 설정하면 됨
sudo vi /etc/httpd/conf/httpd.conf

119번
DocumentRoot "/var/www/html" -> "/var/www/html/wordpress"
=> 외부(클라이언트)에서 웹 브라우저를 통해 IP를 주소창에 입력 시 /var/www/html/wordpress 디렉터리(폴더)에서 index.html 또는 index.php 등을 찾게 됨

131번
<Directory "/var/www/html">을 <Directory "/var/www/html/wordpress">로 변경

151번
AllowOverride None -> AllowOverride All 변경

---
esc -> :wq

sudo systemctl restart httpd
=> 설정값을 변경하였으므로 아파치 웹 서비스를 다시 시작!