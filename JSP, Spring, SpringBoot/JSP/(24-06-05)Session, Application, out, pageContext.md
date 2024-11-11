### Session
- session 내장객체 : 연결 정보를 저장하는 내장객체 (기억장소)
- session 내장객체(기억장소 해제) :
	1. 사용자 브라우저의 세션ID 값 삭제.
	2. 30분 동안 작업(사용자 요청)이 없을 때 session 내장객체 기억장소 삭제.
	3. session 내장객체 삭제하는 명령 seesion.invalidate() => 로그아웃
	4. seesion 은 오브젝트 타입으로 반환되기 때문에 캐스팅해줘야함 
	   (예) boolean isLogin = (boolean) session.getAttribute("name");
```html title:(session.jsp)
<P>세션 ID : <%=session.getId() %> </P>
<P>세션 생성 시간 : <%=session.getCreationTime() %> </P>
<P>세션 최근접근 시간 : <%=session.getLastAccessedTime() %> </P>
<P>세션 유지 시간 : <%=session.getMaxInactiveInterval()%> 초 </P>
<P>세션 유지 시간 변경 (1 => 1800) : <%session.setMaxInactiveInterval(1800);%> </P>

<!-- 로그아웃(연결정보를 삭제) -->
<P>세션 종료 : <%session.invalidate();%> </P>

<!-- 연결 정보를 저장하는 내장객체 (기억장소)에 이름,값 형태로 값을 저장-->
<P>세션 이름,값 저장 : <%session.setAttribute(이름, 값);%> </P>
<P>세션 이름,값 저장된 값 : <%=session.getAttribute("sname")%> </P>


```

### login (Session사용하여 구현해보기)
```html title:JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp2/login.jsp</title>
</head>
<body>
<h3>jsp2/login.jsp</h3>

<!-- 회원가입 form-->
<form action = "loginPro.jsp" method = "post">
	
	<!-- 아이디 입력란 -->
	<div>
		<label for = "id"> 아이디 : </label>
		<input type = "text" id = "id" name = "id" placeholder = "아이디를 입력">
	</div>

	<!-- 비밀번호 입력란 -->
	<div>
		<label for = "pass">비빌번호 : </label>
		<input type = "password" id = "pass" name = "pass" placeholder = "비밀번호 입력">
	</div>
	
	<!-- 전송버튼 -->
	<div>
		<input type = "submit" value = "로그인">
	</div>
</form>

<!-- 로그인 성공여부 판단 -->
<%
if (session.getAttribute("log") != null){
out.println(session.getAttribute("log"));
};
%>
</body>
</html>

```

```html title:JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp2/loginPro.jsp</title>
</head>
<body>
<h3>jsp2/loginPro.jsp</h3>
<%
// http가 들고온 태그정보(파라미터정보 id = 값, pass = 값)를 => 서버 request 기억장소로 저장
// request 에서 id,pass 가져와서 => 변수에 저장
String id = request.getParameter("id");;
String pass = request.getParameter("pass");

%>

<p>폼에서 입력한 ID : <%=id %></p>
<p>폼에서 입력한 PASS : <%=pass%></p>

<%

String dbId = "admin";
String dbPass = "123";

%>

<p>DB에 저장된 ID : <%=dbId %></p>
<p>DB에 저장된 PASS : <%=dbPass %></p>

<%

// 2개 명령중에 어떤 조건에 따라서 2개중에 1개를 선택
//=> 제어문(조건문)
/*
if (조건){
조건이 만족하면 실행할 명령
} else {
조건이 만족하지 않으면 실행할 명령
}
*/

String a;
if(id.equals(dbId) && pass.equals(dbPass)){

//아이디, 비밀번호 일치 => 로그인 표시값을 요청, 페이지 상관없이 값을 유지 필요 저장
// => 세션 기억장소 저장
session.setAttribute("id", id);
response.sendRedirect("main.jsp");
}

else if (!id.equals(dbId)){

a = "아이디 틀림";
out.println(a);
session.setAttribute("log", a);
response.sendRedirect("login.jsp");

}

else {

a = "비밀번호 틀림";
out.println(a);
session.setAttribute("log", a);
response.sendRedirect("login.jsp");

}

%>
</body>
</html>

```

- 로그인 성공시 main.jsp로 이동
```html title:JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp2/main.jsp</title>
</head>
<body>

	<h3>jsp2/main.jsp</h3>
	<P> 로그인 표시값 : <%=session.getAttribute("id") %>님이 로그인 하셨습니다.</P>
	<a href = "info.jsp">나의정보 조회</a>
	<a href = "logout.jsp">로그아웃</a>
	
</body>
</html>
```

- 나의 정보 조회 클릭시 info.jsp로 이동
```html title:JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp2/info.jsp</title>
</head>
<body>

	<h3>jsp2/info.jsp</h3>
	<p>로그인 표시값 <%=session.getAttribute("id") %> 님 정보 조회</p>
	<a href = "main.jsp">메인으로 돌아가기</a>

</body>
</html>
```

- 로그아웃 클릭 시 logout.jsp 로 이동 후 메인으로 리다이렉트
```html title:JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp2/logout.jsp</title>
</head>
<body>

	<h3>jsp2/logout.jsp</h3>
	<%
	//로그아웃 => 연결정보(session 기억장소)를 삭제 => session 기억장소 안에 있는 "id", "admin" 같이 삭제 됨
	session.invalidate();
	
	//main.jsp 로 이동
	response.sendRedirect("login.jsp");
	%>

</body>
</html>
```

### Application
1. 서버 정보를 저장
2. 서버 start 하면 기억장소 만들어지고
3. 서버 stop 하면 기억장소 삭제 되어짐
4. 서버 방문자 수 카운트, ...

```html title:JSP
<p>서버정보 : <%=application.getServerInfo() %></p>
<p>서버물리적 경로 : <%=application.getRealPath("/") %></p>

<!-- 서버가 중지되면 application의 내장객체는 사라진다! session과는 다르다! -->
<p>application 내장객체에 값을 저장 : </p>
<%application.setAttribute("appName", "appValue"); %>

<p>application 내장객체에 저장된 값을 가져와서 출력 : </p>
<p><%=application.getAttribute("appName") %></p>
```

### OUT 
- out 내장객체 : 브라우저에 출력할 내용을 저장
```html title:JSP
<h3>jsp2/out.jsp</h3>

<%
// out 내장객체 : 브라우저에 출력할 내용을 저장
   
// 명령 자체가 틀림!
out.println("브라우저 출력<br>");
out.println("출력 기억장소 크기 : " + out.getBufferSize() + "<br>");
out.println("사용하고 남아 있는 출력 기억장소 크기 : " + out.getRemaining());

// 출력하고 종료 (닫은후 출력하면 오류 발생)
out.close();
//out.println("종료 후 출력<br>");
   
//System.out.println("콘솔창 출력");
%>
```

### PageContext
- pageContext 내장객체 : 현 페이지 정보를 저장하는 내장객체 (기억장소)
1. 페이지가 변경되면 기억장소 삭제 => 새로운 페이지가 만들어지면 기억장소가 만들어짐
```html title:JSP
<p>페이지 정보 : <%=pageContext.getRequest() %></p>

<p>페이지 정보 : <%=pageContext.getResponse() %></p>

<p>페이지 정보 : <%=pageContext.getPage() %></p>

<p>페이지 정보 : <%=pageContext.getSession() %></p>
```