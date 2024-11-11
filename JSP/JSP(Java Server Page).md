- 웹 흐름

![[%EC%9B%B9%ED%9D%90%EB%A6%84.png]]

## 설치방법

- Java (JDK)설치 (Java-11)
- Web Server(Web application Sever) 설치
    - JSP → HTML 변환하는 역할!
- 데이터베이스 서버(DBMS) 설치
- 통합개발툴(IDE ⇒ 이클립스) 설치

**<HTML, CSS, Javascript> ⇒ 웹브라우저 실행가능**

```Plain
<html>
	<head>
		<title>html title</title>
	</head>
	<body>
		<h1>body html</h1>
		body html<br>
		10 + 20
	</body>
</html>
```

**<JSP 파일> ⇒ 웹브라우저 실행 불가능**

```Plain
<html>
	<head>
		<title>jsp title</title>
	</head>
	<body>
		<%="jsp body"%><br> //문자열 출력은 " " 안에 출력
		<%=10+20%>		      //문자열이 아닌 출력
	</body>
</html>
```

- Web Server(Web application Sever) 설치 → TomCat 10 ([https://tomcat.apache.org/download-10.cgi](https://tomcat.apache.org/download-10.cgi))
    
    ![[Untitled 19.png|Untitled 19.png]]
    
    - Server Shutdown port = 끄고 켜고 하는 통로 ! (지정 하지않으면 계속 뜸) = 8005 지정
    - HTTP/1.1 Connector Port = 접속포트 ⇒ 8080 지정
    
    ![[Untitled 1 4.png|Untitled 1 4.png]]
    
    - JVM 위치 지정
- 웹서버 서비스 하는 파일이 들어있는 장소 ⇒ C:\Program Files\Apache Software Foundation\Tomcat 10.1\webapps\ROOT ⇒ i여기 위치에 서비스 하고자 하는 파일을 넣기 !
- jsp파일은 브라우저에선 실행 되지 않았지만 서버를 통하여 실행 하였을때 jsp파일을 → html로 내부적으로 변환하여 출력해준다.
- 웹서버 서비스 시작 start / 종료 stop 의 경로
    
    - C:\Program Files\Apache Software Foundation\Tomcat 10.1\bin
    - Tomcat10w.exe 파일
    
    ![[Untitled 2 4.png|Untitled 2 4.png]]
    
- 웹서버 동작 확인 ⇒ 웹브라우저 URL 서버 주소(자기자신 웹서버 주소 ⇒ ip) ⇒ http://localhost:8080/
    
    ![[Untitled 3 4.png|Untitled 3 4.png]]
    
      
    

## ip 주소

cmd → ipconfig

이더넷 어댑터 이더넷:

연결별 DNS 접미사. . . . :  
링크-로컬 IPv6 주소 . . . . : fe80::5484:44c4:9429:55f4%10  
IPv4 주소 . . . . . . . . . :  
==192.168.1.18==  
서브넷 마스크 . . . . . . . : 255.255.255.0  
기본 게이트웨이 . . . . . . : 192.168.1.254  

  

## jsp 자바 코드

- jsp 파일 생성

```HTML
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%="JSP 본문" %><br>
<%= 100 + 200 %>
</body>
</html>
```

- <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>  
    → JSP 의 설정부분  
    
- <%="JSP 본문" %> **= 문자열 출력**
- <%= 100 + 200 %> = 100 + 200 = 300 이란 결과값 출력

  

- 실행후 결과 코드 보기

```HTML

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSP 제목</title>
</head>
<body>
JSP 본문<br>
300

</body>
</html>
```

- 실행결과 <% %> 부분은 사라져있다!

  


## 내장객체(미리 만들어 놓은 기억 장소)

- request, response, seesion
- pageContext ,application, out, …

## 액션 태그(태그처럼 사용 하도록 만든 문법)

- include

## 쿠키와 세션(session) 공통점, 차이점

## 데이터베이스 서버 연결

- 회원가입, 로그인, 게시판, ….

## MVC(모델2) 프로그램패턴(방식)

## 스프링 프레임워크(MVC)

## 스프링 부트