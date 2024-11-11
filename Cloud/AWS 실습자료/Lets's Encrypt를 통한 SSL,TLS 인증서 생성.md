# Lets's Encrypt를 통한 SSL/TLS 인증서 생성 
1. https://letsencrypt.org
- 인증 절차가 단순해서 단 한 줄 명령어로 인증서 발급 가능
- 발급 대시 시간이 없어 바로 발급
- nginx, apache 와 같은 웹 서버에 맞추어 자동 옵션이 설정되도록 설치 가능
- 인증 유효기간 90일(자동으로 인증 갱신 가능)
- 무료!!!

2. AWS 로그인 -> EC2 대시보드 -> EC2 인스턴스 생성(인스턴스 시작)
- 이름 : SecureWeb
- AMI : Amazon Linux 2 AMI(HVM) - Kernel 5.10
- 인스턴스 유형 : t2.micro
- 키페어 : 기존 것 사용
- 기존 보안 그룹 선택 : ssh, web
- 스토리지 구성 : 기본값(8Gib)
=> 인스턴스 시작

3. MobXterm 원격 접속

4. 아파치 웹 서버 설정
sudo yum install -y httpd
sudo systemctl start httpd
sudo vi /var/www/html/index.html

5. Certbot 설치
- Let's Encrypt 는 ACME 프로토콜을 사용하여 설정한 도메인명의 유효성을 확인하고 인증서를 발급
※ ACME(Automatic Certification Management Environment)
- Let's Encrypt CA(Certificate Authority)로 부터 SSL/TLS 인증서를 발급 받고 싶다면 ACME 클라이언트 소프트웨어를 하나 설치해야 함!
- Let's Encrypt 에서는 그 중 사용하기 편리한 Certbot의 사용을 권장함

sudo amazon-linux-extras install epel -y
sudo yum install -y certbot
certbot --version
certbot --help
=> 발급에 대한 옵션을 볼 수 있음
=> --nginx, --apache, --standalone, --webroot, --manual 등이 있음

6. Let's Encrypt SSL 인증서 발급
1) webroot
2) 사용하는 웹서버의 인증서 발급
3) Standalone
4) DNS 

1) webroot로 SSL 인증서 발급
- 사이트 디렉터리 내에 인증서 유효성을 확인할 수 있는 파일을 업로드하여
  인증서를 발급 받는 방법
- 서버 내에 .well-known 디렉터리가 있을 곳을 지정
- 1) 특정 폴더를 만들고
  2) 폴더에서 letsencrypt.conf 파일을 만들고
  3) 이를 웹 서버 설정 파일에서 읽어옴

sudo mkdir -p /var/www/letsencrypt/.well-known/acme-challenge
=> -p 옵션은 중간부분의 디렉터리가 없으면 생성하면서 최종적으로 acme-challenge 디렉터리를 생성

sudo certbot certonly --webroot --webroot-path=/var/www/letsencrypt -d s31.itwillbs.com -m jskang@itwillbs.co.kr

---
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.4-April-3-2024.pdf. You must agree in
 order to register with the ACME server. Do you agree? 
 
 ---
(Y)es/(N)o: y 

---
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.

---
(Y)es/(N)o: y

에러 발생!
해당 경로에 .well-known  과 관련된 파일이 없다는 404 not found 에러가 발생!

에러를 해결하기 위해
1) 왼쪽의 Sessions -> secure_web 더블클릭 -> 새로운 접속탭을 하나 생성
2) [첫번째 접속 탭]에서
sudo certbot certonly --manual --email jskang@itwillbs.co.kr -d s31.itwillbs.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Create a file containing just this data:

8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g.XDHaQWXHyEHu9VAPUUHqHzhwl0ckMiJMU_7yqtH3wEo

And make it available on your web server at this URL:

http://s31.itwillbs.com/.well-known/acme-challenge/8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue

=> create 아래의 내용을 메모장에 복사8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g.XDHaQWXHyEHu9VAPUUHqHzhwl0ckMiJMU_7yqtH3wEo 

3) [ 두 번째 접속탭 ] 으로 이동
sudo -sE
cd /var/www/html
mkdir -p .well-known/acme-challenge
ls -a
=> .well-known 디렉터리 확인
cd .well-known/acme-challenge
sudo vi 8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g

'i'를 눌러 INSERT 모드로 변경하고 나머지 모두를 내용으로 붙여넣기
8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g.XDHaQWXHyEHu9VAPUUHqHzhwl0ckMiJMU_7yqtH3wEo
esc -> :wq

웹 브라우저에서 http://s31.itwillbs.com/.well-known/acme-challenge/8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g
=> certbot이 원하는 경로 http://s31.itwillbs.com/.well-known/acme-challenge/8m6IX9PYa6Cy78EZShX92JgYQRj8bA03qfPwxXaMT3g 의 파일이 생성되었으므로 data를 확인해서 인증서를 발급해준다!

4) [ 첫번째 접속 탭]으로 다시 이동
Press Enter to Continue 화면에서 엔터키를 입력!

---

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/s31.itwillbs.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/s31.itwillbs.com/privkey.pem
   Your certificate will expire on 2024-12-26. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
=> Let's Encrypt에서 인증서 발급됨!

7. Apache에 추가 설정
sudo vi /etc/httpd/conf/httpd.conf
set nu -> Shift + G 맨 아래 라인으로 이동

362라인에서 'o'를 눌러서 빈 라인을 입력하면서 INSERT 모드로 전환

```
364 <VirtualHost *:80>
365     DocumentRoot /var/www/html
366     ServerName itwillbs.com
367     ServerAlias s31.itwillbs.com
368 </VirtualHost>
369 <VirtualHost 13.125.73.234:443>
370     DocumentRoot /var/www/html
371     ServerName s31.itwillbs.com
372
373     SSLEngine on
374     SSLCertificateFile /etc/letsencrypt/live/s31.itwillbs.com/cert.pem
375     SSLCertificateKeyFile /etc/letsencrypt/live/s31.itwillbs.com/privkey.pem
376     SSLCACertificateFile /etc/letsencrypt/live/s31.itwillbs.com/fullchain.pem
377 </VirtualHost>
```

---------------------
esc -> :wq

Apache에서 SSL 접속 시 참고(사용)하는 설정 파일
/etc/httpd/conf.d/ssl.conf

sudo yum install -y certbot python2-certbot-apache

ssl.conf에서 지정된 위치가 있으므로 let's encrypt를 통해 다운로드 받은 인증서를 지정된 위치로 복사

sudo cp /etc/letsencrypt/live/s31.itwillbs.com/cert.pem /etc/pki/tls/certs/cert.crt
sudo cp /etc/letsencrypt/live/s31.itwillbs.com/privkey.pem /etc/pki/tls/private/privkey.key
sudo cp /etc/letsencrypt/live/s31.itwillbs.com/fullchain.pem /etc/pki/tls/certs/

sudo vi /etc/httpd/conf.d/ssl.conf
100 SSLCertificateFile /etc/pki/tls/certs/localhost.crt -> SSLCertificateFile /etc/pki/tls/certs/cert.crt 로 변경

107 SSLCertificateKeyFile /etc/pki/tls/private/localhost.key -> SSLCertificateKeyFile /etc/pki/tls/private/privkey.key

---

esc -> :wq

8. 마지막으로 아파치 웹 서비스 재시작
sudo systemctl restart httpd

9. EC2 보안그룹 -> web -> 인바운드 규칙 추가 -> HTTPS 포트번호 : 443(자동 입력, 변경 불가), 사용자 지정 : anywhere-IPv4 (0.0.0.0/0, 모든 IP 허용)

10. 웹브라우저에서 https://s31.itwillbs.com 으로 확인!!




