# 리눅스 기반 워드프레스(WordPress)
- 워드프레스는 홈페이지를 쉽게 제작할 수 있는 환경
- 웹 서비스 프로그램, 데이터베이스 서버, php 프로그램 언어 필요
- -> 웹 서비스 프로그램 : Aapche
- -> 데이터베이스 서버 : mariadb
- -> 언어 : PHP

## 시작전 확인
1. 프로그램 설치 확인 (rpm -qa httpd php mariadb-server)
- 아무것도 나오지 않으면 설치가 되지 않은 상태

2. 가상머신(VM)의 ip주소 확
 - enp0s3 : 10.0.0.2.15/24 
 
## 워드프레스 설치
 1. 가상머신 업데이트 (yum -y update)
 2. 워드프레스를 동작하기 위한 프로그램(=패키지)를 설치
     (yum -y install httpd php php-mysqlnd mariadb-server)
3. 패키지 설치 확인 (rpm -pa httpd php mariadb-server)
4. systemctl status httpd -> Active =(dead)
5. systemctl start httpd -> Active = (runnig)
6. 리눅스 재시작 시 자동 웹 서비스 동작
   -> systemctl enable httpd

## 리눅스 웹 설정
1. cd /var/www/html -> 폴더변경
2. gedit index.html (= gedit/var/www/html/index.html)
   GUI 환경에서는 문서편집기 gedit를 사용하여 index.html 생성
   간단한 내용을 입력
   만약 , TUI 환경이라면 'vi' 'nano' 를 이용해서 생성

## 웹 페이지 확인
1. 프로그램 -> 즐겨찾기 -> FireFox -> 주소창에 localhost 또는 127.0.0.1 입력 
   => index.html 내용 확인

## 윈도우에서 웹 동작 확인
- 윈도우 -> 리눅스 웹서버로 페이지를 요청하는 경우 리눅스 서버의 http 서비스가 허용 되어 있어야 함
- 터미널 firewall-config
- 설정 : 런타임 -> 영구적
- 서비스 : \[ v ]  http
- 옵션탭 -> firewall 다시 불러오기
- systemctl list-unit-files (리눅스 시작시 자동으로 켜지는 프로그램 확인)

## 데이터베이스 설정
- mariadb 실행  -> systemctl start mariadb

## PHP 언어 동작 확인
<? php
	phpinfo();
?>
- 웹과 관련된 설정이 변경되면 웹 서비스 재시작
1. 어댑터 브리스 -> 192.168.1.x/phpinfo.php 로 접속
2. NAT -> localhost:8000/phpinfo.php 로 접속
3. 또는 -> 127.0.0.1:8000/phpinfo.php 로 접속
4. 호스트 전용 어댑터 -> 192.168.56.x/phpinfo.php 로 접속
- PHP Version 내용이 나오면 서버가 PHP 언어를 사용할 수 있는 환경을 갖춤

## 워드프레스가 사용할 데이터베이스 설정
- mysql -u root -p
- Enter password : 따로 설정한적이 없으므로 엔터 접속
- root -> mariadb 관리자 계정을 지칭
- show databases; -> mariadb의 데이터베이스 이름 확인
- database -> schema 라고 하며 내부에 0개 이상의 테이블을 가지고 있음.

- create database wpDB;
	->워드프레스가 사용할 데이터베이스 wpDB를 생성
- grant
	- grant \[권한] on \[사용할 데이터베이스명] to \[유저명] identified
	- grant all privileges on wpDB.* to wpUser@localhost identified by '1234'
	- 권한을 설정하는 명령어 <-> revoke
	- 권한(CRUD)
	->wpDB가 워드프레스가 사용할수 있도록 설정 
	->1234 패스워드를 가진 wpUser 를 생성. wpuser는 wpDB의 모든 테이블에 대해서 읽고,쓰고,수정,삭제하는 권한을 가짐
- flush privileges;
- exit 데이터베이스 종료

## 워드프레스 설치
- wget https://ko.wordpress.org/wordpress-4.9.6-ko_KR.tar.gz
	-> wget = 웹에서 가져오겠다! 라는 의미
	-> .tar.gz 는 압축된 파일
	-> tar -xvfz wordpress-4.9.6-ko_KR.tar.gz (=w 탭 자동완성)
	-> 압축해제를 하면 wordpress라는 폴더(디렉터리)가 생김
	
- chown -R apache.apache wordpress
- chown -> 소유자 변경
- -R : wordpress 내의 모든 디렉터리(폴더)와 파일들의 소유자를 변경하는 옵션
- -R : 을 적지않으면 (wordpress)폴더에만 적용 . wordpress 아래 의 다른 디렉터리 및 파일에 대해서는 변경되지 않음
- apache . apache : 소유자 , 소유그룹 표시

- 외부 (윈도우 등)에서 웹 브라우저를 통해서 wordpress 폴더에 접근할 경우
- wordpress 디렉터리(폴더) 는 apache 소유여야 한다.

- chmood 777 wordpress
\[허가권자 permision]
- 리눅스 운영체제는 파일에 대해서 소유자가 존재하고 유저에대한 접근 권한이 존재
- 접근권한 : r(일기), w(쓰기), x(실행)
- 3개씩 끊어서 소유자 / 소유그룹/ 그밖의 사람 구성 총 9요소
- rwxr-xr-x : 소유권자에 대해서 읽고 쓰고 실행, 소유그룹 읽기 실행, 그밖의 사람의 읽기 실행 권한이 부여됨
	-> 소유자만 읽고 쓰고  실행하고 싶다면
	->chmod 700 \[파일명 또는 디렉터리명]
	->rwx------
	-> 또 다른 설정방법 소유자(u), 소유그룹(g), 그 밖의 자(O) 에 대해서
	-> + 또는 - 기호를 사용하요 r w x 권한을 부여 가능
	ex) rwxr-xr-x 에서 r-xr-xr-x 로 변경하려면 즉, 사용자에게 쓰기 권한을 빼려면
     chmod u-w \[파일명]
	ex) 모든 사용자에게 w권한을 부여할 때는 chmod a+w \[파일명]
	ex) 모든 사용자에게 읽고, 쓰고, 실행하는 권한을 부여하려면 chmod a+rwx \[파일명]


- cd wordpress 
- ls
  -> 여러파일, 폴더디렉터리(폴더)가 보인 = 워드프레스 설정파일 이름 wp-config.php 이다.
  ->이 파일이 존재하지않음
  ->wp-config-sample.php 를 복사해서 wp-config.php 를 생성해야하함
  -> cp \[무엇을(원본)] \[~로(복사본)] 경로를 지정해야하지만 현재위치가 /var/www/html/wordpress 라면 생략가능
  -> cp wp-config-sample.php wp-config.php 로 복사가능 
  -> wp-config.php 가 복사되어있음!
  -> 복사된 wp-config.php 파일을 수정
  -> 23번행 define('DB_USER', 'username_here'); -> wpDB로 변경
  -> 26번행  define('DB_USER', 'username_here'); -> wpUser로 변경
  -> 29번행 define('DB_PASSWORD', 'password_here'); -> 1234로 변경
  
- 워드프레스 설치 완료 후 홈페이지 생성완료
- 현재 wordpress는 서버ip주소/wordpress로 접근해야함
- 주수창에 서버ip주소만 입력하면 wordpress 화면이 보이도록 설정

- 외부에서 웹 브라우저로 접속하면 documentRoot 설정값에 의해서
- /var/www/html 안의 파일을 찾음

- 아파치 웹서비스 설정 값을 변경
- 아파치 설정 파일 위치 /etc/httpd/conf/httpd.conf <<
- vi /etc/httpd/conf/httpd.conf
- 열어서 119번행  DocumentRoot "/var/www/html" << 초기설정값
	-  DocumentRoot "/var/www/html/wordpress" <<값 변경
- 131번 행 <Directory "/var/www/html"> << 초기값
	- <Directory "/var/www/html/wordpress">
- AllowOverride None << 초기값 
	- None -> All 로 변경
- systemctl restart httpd << 재시작
- 

