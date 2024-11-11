# AWS EC2(nginx) - RDS(MySQL) 연동 워드프레스 구현
1. EC2 대시보드 -> 인스턴스 -> 인스턴스 -> 인스턴스 시작
- 이름 : nginx
- AMI : Amazon Linux 2
- 인스턴스 유형 : t2.micro
- 키페어 : 기존의 것 사용
- 기존 보안 그룹 선택 : ssh, web
- 스토리지 : 기본값(8GiB)
=> 인스턴스 시작 , 모든 인스턴스 보기

2. 생성된 nginx의 퍼블릭 IPv4 주소를 복사해서 원격 접속

3. nginx 공식 리포지토리 추가
- yum 명령어 시 EC2 인스턴스가 저장(Repository)를 찾는데 그 경로가 /etc/yum.repos.d/ 이다.
- 여기에 있는 저장소를 기본값으로 찾게 됨
- AWS가 아닌 일반적인 리눅스 라면 /etc/yum/repo.d/centos-base.repo 파일에 저장된 기본값으로 저장소를 찾음
sudo vi /etc/yum.repos.d/nginx.repo
- 원하는 버전의 nginx를 설치할 수 있음
---
# nginx
- name=nginx repo
- baseurl=http://nginx.org/packages/centos/7/$basearch/
- gpgcheck=0
- enabled=1
---
esc -> :wq

4. nginx 설치
sudo yum install -y nginx

5. nginx 시작
sudo systemctl start nginx

6. nginx 동작 확인
sudo systemctl status nginx

7. 서버가 재시작 시 자동으로 nginx가 동작 설정
sudo systemctl enable nginx

8. 윈도우 웹 브라우저에서 동작 확인
주소창에 EC2 인스턴스의 퍼블릭 IPv4 주소를 입력

9. 가상 호스트 사용
- AWS EC2 인스턴스를 여러 도메인으로 공개하기 위한 구조
- 1) sites-available 디렉터리 생성 및 virtual.conf 파일 생성
  2) 로그 파일이 위치할 디렉터리 생성
  3) HTML 파일이 위치할 디렉터리 생성(DocumentRoot)
  4) nginx.conf 편집

1) sites-available 디렉터리 생성 및 virtual.conf 파일 생성
- cd /etc/nginx
- sudo mkdir sites-available
- sudo vi sites-available/virtual.conf
---

```
server {
    # 서버 IP 주소에 80번 포트로 오픈할 것을 선언
    listen 80;
    # 오픈할 웹 서버의 도메인을 설정(도메인이 없으면 localhost로 설정)
    server_name localhost;
    # 엑세스 로그 남길 파일 경로를 지정
    access_log /var/log/nginx/virtual/access_log;
    # 오류 로그 남길 파일 경로를 지정
    error_log /var/log/nginx/virtual/error_log;

    location / {
	# HTML 파일이 위치할 도큐먼트 루트를 설정
	root /var/www/virtual;
	# 사이트의 TOP 페이지로 할 파일을 설정
	index index.php index.html;
    }
}
```
---
esc -> :wq

2) 로그 파일이 위치할 디렉터리 생성
sudo mkdir /var/log/nginx/virtual

3) HTML 파일이 위치할 디렉터리 생성(DocumentRoot)
sudo mkdir -p /var/www/virtual
※ -p : /var/www 가 없으면 www를 만들고 그 아래에 virtual 생성

4) nginx.conf 편집
sudo vi /etc/nginx/nginx.conf
========================
:set nu

31번 라인 주석처리
- \#include /etc/nginx/conf.d/*.conf;

32번 라인 추가
- include /etc/nginx/sites-available/*.conf;
---
esc -> :wq

10. 설정 파일이 올바른지 확인
- sudo nginx -t
---------------
- nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
- nginx: configuration file /etc/nginx/nginx.conf test is successful
---

11. index.html 만들기
sudo vi /var/www/virtual/index.html
=> 간단한 내용 입력

12. nginx 재시작
sudo systemctl restart nginx

13. 윈도우 웹 브라우저에서 index.html 내용 확인

14. php 설치
- nginx는 아파치와는 달리 php만 설치하였다고 해서 바로 사용할 수 없다!
- php-fpm을 이용해 php 다룰 수 있다.
- AWS에서는 amazon-linux-extras 에서 제공하는 php를 설치할 수 있음
- 아마존에서 제공하는 php8.1을 설치

sudo amazon-linux-extras 
=> 66번 항목에 php8.1이 보임

sudo amazon-linux-extras enable php8.1
=> php8.1 항목을 enable(활성화)하여 php를 설치할 때 php8.1이 기본값으로 선택되도록 설정

sudo yum -y install php php-fpm php-cli php-common php-devel php-pear php-mbstring php-mysqlnd php-pdo php-gd
=> php 관련 파일 설치

which php-fpm
=> php-fpm 위치 확인
=> /usr/sbin/php-fpm
=> 설정파일 위치 : /etc/nginx/conf.d/php-fpm.conf

15. php-fpm.conf 설정
- /etc/nginx/nginx.conf 파일에서 include /etc/nginx/conf.d/*.conf; 라인을 주석처리(#)하여
  php-fpm.conf 파일이 설정파일에 포함되지 못함!
- 따라서 php-fpm.conf 파일을 /etc/nginx/sites-available/ 디렉터리(폴더)에 복사해야 함!

sudo cp /etc/nginx/conf.d/php-fpm.conf /etc/nginx/sites-avaiable/
=> yum 명령어로 php-fpm을 설치하면 설정파일 php-fpm.conf 파일이 주석처리 해놓은
/etc/nginx/conf.d 디렉터리(폴더)에 위치하게 됨
=> 주석처리로 인해 php-fpm.conf 파일을 접근할 수 없음!!
=> 따라서 새롭게 include 한 /etc/nginx/sites-avaiable 디렉터리로 복사

16. php-fpm 실행
sudo systemctl start php-fpm

17. php-fpm 자동 실행 설정
sudo systemctl enable php-fpm

18. nginx와 php의 연동을 위해 설정파일 수정
sudo vi /etc/nginx/sites-available/virtual.conf
---
```
server {
    listen 80;
    server_name web.itwillbs.com;
    
    root /var/www/virtual;
    index index.php index.html;
    charset UTF-8;

    location ~ \.php$ {
        try_files $uri =404;

        include fastcgi_params;
        fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_index index.php;
        # fastcgi_intercept_errors on;
        # fastcgi_keep_conn on;
        # fastcgi_read_timeout 300;

        # fastcgi_pass   127.0.0.1:9000;
        fastcgi_pass  unix:/var/run/php-fpm/www.sock;
        #for ubuntu unix:/var/run/php/php8.0-fpm.sock;

        ##
        # FastCGI cache config
        ##

        # fastcgi_cache_path /var/cache/nginx levels=1:2 keys_zone=WORDPRESS:10m max_size=1000m inactive=60m;
        # fastcgi_cache_key $scheme$host$request_uri$request_method;
        # fastcgi_cache_use_stale updating error timeout invalid_header http_500;        
        
        fastcgi_cache_valid any 30m;
    }
}
```

---
※ location / {} 부분 삭제, location ~ \.php$ {} 추가
esc -> :wq

19. 도큐먼트 루트에 php 파일 생성
sudo vi /var/www/virtual/info.php
---
<?php
    phpinfo();
?>
---
esc -> :wq

20. nginx 재실행
sudo systemctl restart nginx

21. 윈도우 웹 브라우저 주소창에 AWS EC2 IP주소/info.php 입력해서 확인

---

#  RDS(MySQL)을 연동하여 wordpress 설치
22. RDS를 MySQL로 구현할 예정이므로 mysql 클라이언트 설치
sudo yum install -y mysql

23. 도큐먼트루트로 이동
cd /var/www/virtual

24. wordpress 설치
sudo wget https://wordpress.org/latest.tar.gz

25. 압축 해제
sudo tar xvfz latest.tar.gz

26. wordpress의 소유자를 nginx로 변경
sudo chown -R nginx.nginx /var/www/virtual/wordpress

27. rds 대시보르도 이동, rds 데이터베이스 설치
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
- 마스터 사용자 이름 : admin
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

28. wordpress의 wp-config.php 생성
cd /var/www/html/wodpress
sudo cp wp-config-sample.php wp-config.php

29. wp-config.php 수정
sudo vi wp-config.php
---
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'admin' );
define( 'DB_PASSWORD', 'class10522' );
define( 'DB_HOST', 'itwillbs-rds-class1-team1.cxuw4m6kgp6q.ap-northeast-2.rds.amazonaws.com' );
-  새로 생성한 Amazon RDS의 엔드포인트를 복사해서 'localhost' 자리에 붙여 넣음!
---
esc -> :wq

30. nginx 웹 서비스 재시작
sudo systemctl restart nginx

31. 윈도우 웹 브라우저에서 EC2 인스턴스 IP주소/wordpress 를 입력하면 워드프레스의 첫 설치 과정으로 넘어감!

