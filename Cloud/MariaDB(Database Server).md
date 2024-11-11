- #####  mariaDB 설치
	- 마리아 디비 설치 확인 (rpm -qa maria*)
		- mariadb-libs-5.5.68-1.el7.x86_64
	- 마리아 DB관련 파일을 미리 확인
		- yum list maria*
	- 그 중 클라이언트용 mariadb 와 서버용 mariadb-server 패키지 필요
		- yum install -y mariadb mariadb-server
		- 서비스를 위해서 mariadb-server를 설치하고, DB서버에 접속하는 용도의 클라이언트 프로그램 mariadb 까지 설치
		- ststemctl enable mariadb (리눅스 시작시 자동 시작)
	- mariadb 접속 (mysql -u root -p) -> 비밀번호 없음
	- 디비 나가기 exit
---
- ##### mariadb 초기 보완설정
	- 처음 설치하자 말자 해주는게 좋음
	- mysql_secure_installation
	- Enter current password for root (enter for none):
	  -> 현재 설정된 root의 비밀번호(초기시 설정값 없으면 엔터)
	  -> Set root password? [Y/n] 지금 만들기 (Y)
	  -> Remove anonymous users? [Y/n]  익명사용자를 삭제하겠습니까?
	  (Y)
	  ->Disallow root login remotely? [Y/n]  관리자 계정인 root의 원격접속을 거부하는 설정 원격접속을 거부시키려면 y 허용하려면 n를 입력
	  -> Remove test database and access to it? [Y/n] 테스트데이터베이스 를 쓰지않는다면 삭제
---
 - ##### mariaDB 설정
	 - 설정파일 : /etc/my.cnf
	 - 마리아DB를 처음 설치하면 생성되는 초기의 my.cnf파일은 내용이 적음
	 - 마리아DB에서 제공하는 샘플 설정 파일을 복사해서 원하는 내용 수정
	 - 샘플파일 : /usr/share/mysql
	 - 파랑색 디렉터리는 캐릭터셋(언어)
	 - .sql : 스키마 파일 데이터베이스가 기본적으로 사용하는 구성 파일
	 - .cnf: 설정파일
	 - my-small.cnf / my-medium.cnf / my-large.cnf
		-  파일의 내용은 같지만 캐시 메모리 부분의 설정이 다름
		-  small : 256MB , medium : 512MB, large : 1 ~ 2 GB
	-  cp my-small.cnf /etc/my.cnf : 원본을 ~로(복사본) 으로 이동
	- 같은이름으로 복사할때는 파일명을 생략
	- cp: overwrite /etc/my.cnf? -> 이미존재하는 파일명인데 덮어쓸것인지? (Y)

	- 마리아DB 설정 파일을 수정
	- 앞의 cp명령어로 초기의 my.cnf파일이 아닌 복사한 파일이 열림
	- 22 default-character-set = utf8 
	  -> 기본언어셋 utf8 로 셋팅 
	  -> 마리아DB 기본 언어셋은 '라틴1'으로 설정되어있음
	- 30 skip-name-resolve
	  -> 쿼리 구문 실행 시 서버가 사용자의 소속을 알아보는 과정을 생략
	  -> 빠른 성능을 위해서 설
	- 31 character-set-server = utf8
---
- ##### 실습환경 구성
	- create database shopping_db;
	- show databases;
	- use shopping_db;

	- create table customer(
	    -> id varchar(20) not null,
	    -> name varchar(20) not null,
	    -> age int(11) null,
	    -> address varchar(100) not null,
	    -> primary key(id));
	
	- create table purchase(
	    -> no int(11) not null primary key auto_increment,
	    -> cust_id varchar(20) not null,
	    -> date datetime not null,
	    -> product varchar(10) not null);
	
	- \[사용자추가]
		- grant all  on shopping_db.* to itwill@localhost identified by '1234';
		- grant all : 모든 권한 부여 (CRUD)
		- on shopping_db.* : shopping_DB의 전체 테이블
		-  itwill@localhost  : 아이티윌로 접속하는 계정
		-  identified by '1234'; : 비밀번호를 1234로 지정
	
	- 생성된 itwill 계정으로 마리아DB 접속
		- show databases;
		- mysql 데이터 베이스가 보이지 않는다
		- 루트계정만 접근할수 있고 나머지는 접근할수 없음
		- drop user itwill@localhost; -> 유저라서 뭔가 할수없다
		- 사용자 추가삭제는 root 유저만 할수 있음
		- exit
---
- ##### 외부에서 마리아 DB 접속하기(리눅스 -> 마리아DB서버)
	- *web*
	- 마리아 DB 설치 (yum install -y mariadb)

	- DB서버로 원격 접속
	- mysql -u 유저명 -h DB서버 ip주소 -p 비밀번호
	  ->  mysql -u itwill -h 192.168.56.103 -p
	  -> 에러 이유
	  1) DB서버의 방화벽이 동작하고 있고, mysql을 허용하지 않았다면 방화벽 설정필요!
	  2) 마리아DB 서버 내의 itwill@클라이언트ip주소 의 grant 명령어 필요

	- *DB*
	1) 방화벽 설정(터미널) - firewall-cmd --add-service=mysql --permanent
	   -> firewall-cmd --reload
	2) web의 ip주소인 192.168.56.102의 주소를 가지는 장치에서 접속하는 itwill 유저에 대한 권한 설정이 필요!
	   -> 유저를 추가하기 위해서는 root 권한 필요
	   -> grant all on shopping_db.* to itwill@'192.168.56.102' identified by '1234';
	   -> 192.168.56.102 ip 주소를 가지는 장치에서 itwill 계정으로 접속 허용  
---
- ##### 외부에서 마리아DB 접속(윈도우 -> 마리아DB서버)
	- *DB*
	- grant all on shopping_db.* to itwill@'192.168.56.1' identified by '1234';

	- *윈도우*
	- 워크벤치 실행
	- '+'버튼 클릭 - > setup new connection 설정
	- Connection Name : MariaDB
	- Hostname : 192.168.56.103
	- UserName : itwill
	- Password : '1234'
---
- ##### 데이터베이스 유저 확인
	- mysql -u root -p mysql
	- select user, host from user where user not like '';
	  -> user테이블의 user컬럼의 공백을 제외하고 user 컬럼과 host만 묶어서 보여주기
	  -> 위에서 설정한 webserver와 윈도우의 ip주소를 host로 가지는 itwill 계정 확인
---
- ##### root 비밀번호 설정
	 1)  리눅스에서 설정하는
		    -> mysqladmin -u root -p password '1111'
		    -> 기존비밀번호 1234
		    -> 변경완료
	    
	 2)  마리아DB 내에서 설정(권장)
		    -> use mysql
		    -> update user set password = password('1') where user = 'root';
		    -> flush privileges;
---
- ##### 데이터베이스 백업 
	1) mysqldump -u root -p --all-databases;
	   ->mysqldump : 지정된 데이터 베이스 또는 테이블의 내용을 화면(모니터)에 출력
	- 백업이라고 하면 보통은 파일 형태로 존재!
	- 모니터에 출력되는 내용을 파일형태로 저장하는 과정이 필요!
	- 리다이렉션을 통해서 이를 구현
		->리다이렉션(redirection) - 출력의 방향을 바꿔주는 방법(기술)
		-> 리눅스의 표준 출력장치(stdOut)는 모니터
		->  리다이렉션 기호 : <<, < , >, >>
		
	2) 특정 데이터베이스(스키마) 만 백업을 하려고 하면
		-> mysqldump -u root -p \[데이터베이스명] > 백업파일이름.sql
		
	1) 특정 데이터베이스의 특징 테이블 하나만 백업하려면
		-> mysqldump -u root -p \[테이터베이스명]\[테이블명] > 백업파일명.sql 
---
- ##### 리다이렉션 실습
	- echo 1  : 1을 모니터에 출력
	- touch test.txt : 비어 있는 파일 (빈파일)을 만드는 명령어
	- echo 1 > text.txt
	  -> 1을 모니터에 출력하지 않고, text.txt로 방향을 바꿔서 보냄
	  -> text.txt 에 1이 입력됨
	- echo 1 >> text.txt
	  -> 기존내용 삭제하지 않고 1만 마지막 라인에 추가
	- '>', '>>' :
		- 공통점 : 표준출력인 모니터로 출력하지 않고, 파일로 출력하는 (입력) 리다이렉트 기호
		-  차이점 : '>' 기존 파일의 내용을 없애고 파일에 출력
		  '>>' 삭제하지않고 마지막 라인에 추가
---
- ##### 데이터베이스 복구
	- 데이터베이스 복구 시 리다이렉션 '<' 사용
	- customer_table_backup.sql 파일을 활용하여 customer 테이블의 복구 실습
	- customer table 복구
		-> mysql -u root -p shopping_db < customer_table_backup.sql
---
- ##### MariaDB 데이터베이스 서버의 이중화 (데이터가 없는 초기 상태에서 DB 이중화를 하는것이 가장 이상적)
	- 서버의 이중화란? 하나의 데이터베이스 서버와 똑같은 내용을 가진 데이터베이스 서버를 운영하는 것
	- Master, slave 개념 사용
	- Master 로 설정된 데이터베이스에서 데이터베이스(스키마) 또는 테이블을 생성하면 slave 데이터베이스 에 자동으로 생성이 된다.
	- 두 대의 리눅스 서버가 필요
	- ##### WebServer(가상머신)
		- 기존에 사용하던 WebServer를 Slave로 동작
		- 샘플파일(my-small.cnf)을 마리아DB 설정파일(my.cnf)로 복사
		- 기존 의 파일이 있으므로 덮어씀
		- my.cnf 파일의 server-id  = 1 (Master) -> 2(Slave)로 변경
		- log-bin=mysql-bin -> 주석해제
		- binlog_format=mixed -> 주석해제
		-  max_binlog_size = 1000M -> 작성
		-  expire_logs_days = 14 -> 작성
	
	- ##### Master로 인식될 DB 가상머신으로 이동
		- log-bin=mysql-bin -> 주석해제
		- binlog_format=mixed -> 주석해제
		- max_binlog_size = 1000M -> 작성
		- expire_logs_days = 14 -> 작성
		- grant replication slave on *.* to slaveroot@(webserver 가상머신ip주소) identified by '1234';
		- show master status (표 형식으로 Master의 상태를 보여줌)
		  -> file : mysql-bin.000001
			  ->MariaDB 시작할때 마다 파일명이 변경
		  -> position : 474
			  -> SQL 쿼리를 처리하면 변경
	- ##### WebServer(가상머신)
		- firewall-config
		- 런타임 -> 영구적 mysql 체크
		- 옵션 -> firewall 다시불러오기

		- 디비접속후 
		  -> change master to master_host='Master DB의 ip주소',
		  master_user='slaveroot',
		  master_password='1234',
		  master_port=3306,
		  master_log_file='mysql_bin.000001',
	- ##### \[이중화 확인]
		- DB('Master')에서 create database MasterDB; 명령어로 데이터베이스를 생성하면 Slave DB(WebServer)에 같이 만들어짐




