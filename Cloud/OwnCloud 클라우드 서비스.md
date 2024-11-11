- 터미널 ip add 192.168.56.102
1. 웹서비스 프로그램 
2. php 
3. 데이터베이스
- -> 3개의 프로그램 설치 yum install -y httpd php mariadb-server
- cd -> /var/www/html/
- vi index.html -> systemctl start httpd -> systemctl enable httpd
- 방화벽설정(http , https)
	-  firewall-cmd --add-service=http --permanent (http 열었음)
	-  firewall-cmd --add-service=https --permanent (https 열었음)
	-  firewall-cmd --reload (방화벽 재시작)
- 원클라우드 zip 파일 다운로드
-  wget \https://download.owncloud.com/server/owncloud-10.11.0.zip
- 압축풀기 -> unzip -q owncloud-10.11.0.zip
- chown -R apache.apache owncloud 소유권한 그룹 아파치로 변경
- chmod  -R 755 owncloud -> chmod 변경
- \http://192.168.56.102/owncloud/index.php -> PHP 버전 업그레이드 필요
-  yum remove -y php*  php-\*-> php 파일 전부다 지우기
-  cd /etc/yum.repos.d/ -> 로 이동
- yum install -y epel-release ->기존 저장소가 아닌 확장된 저장소를 찾기 위한 업데이트
- yum install -y \https://rpms.remirepo.net/enterprise/remi-release-7.rpm -> PHP 7.x 버전의 패키지를 포함하는 저장소를 추가적으로 등록
- 이제 yum 명령어를 통하여 PHP 7.x 이상의 버전도 설치 가능
- yum-config-manager --enable remi-php73 ->설치할때 7.3버전이 기본값
- yum.repos.d]# yum install -y php php-common php-fpm -> php 관련 설
- yum install -y php-mysqlnd php-pecl-memcache php-pecl-memcached php-gd php-mbstring php-mcrypt php-xml php-pecl-apc php-cli php-pear php-pdo php-curl php-intl php-zip
- systemctl restart httpd
- 웹브라우저에  \http://192.168.56.102/owncloud/index.php -> 실행시 원클라우드 화면은 등장하지만 DB 설정

- ownCloud 사용할 DB설정
- systemctl start mariadb
- create database webDB; -> 디비 생성 , show databases; -> 디비생성확인
- GRANT ALL ON webDB.* TO webUser@localhost IDENTIFIED BY '1234';
  -> 암호 1234 를 가지는 webUser에게 webDB 데이터베이스 공간의 모든 권한을 주겠다!
- flush privileges; -> 권한변경 적용
- exit -> setenforce 0 (setenforce 끄)




