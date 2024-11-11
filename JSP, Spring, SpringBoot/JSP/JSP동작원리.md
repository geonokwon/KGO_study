- 웹 서버(웹 애플리케이션 서버) 동작 원리
- test1.jsp에서 사용자가 입력한 아이디, 좋아하는 숫자를 => http가 서버에 전달
    - ⇒ 웹서버 : testPro1.jsp 페이지 찾음
    - ⇒ jsp 파일 처리 필요 판단 => 웹 애플리케이션 서버에 전달
    
- ⇒ 웹 애플리케이션 서버
    - ⇒ 1) 서버에서 기억 장소를 만듬 (request, response 기억 장소) ⇒ request 기억 장소에 HTTP가 가져온 사용자가 입력한 아이디(id = kim), 좋아하는 숫자를 저장(num = 5) 를 저장
    - ⇒ 2) web.xml 참조
    - ⇒ 3) 개발자가 명령 ⇒ 처리하는 처리 담당자(서블릿)가 처리 ⇒ 응답 파일 html 만들어 줌⇒ reponse 기억 장소 응답 파일을 저장 ⇒ 웹 서버 ⇒ HTTP에 전달 ⇒ 사용자 화면에 전달
- 개발자 명령
    - request 기억 장소에 HTTP가 가져온 사용자가 입력한 아이디(id = kim), 좋아하는 숫자를 저장 ⇒ request 기억 장소에서 id = kim, num = 5 값을 가져와서 출력
    - request기억 장소에 가서 . 참조(찾는다)한다 ⇒ 기억 저장소에 태그 이름을 불러서 가져오는 명령 ⇒ request.getParameter("태그 이름")
    - 클라이언트(사용자)와 웹서버 연결 종료, request, response 기억장소 해제, 처리담당자(서블릿) 해제
- HTML에서 form 태그로 서버로 넘겨줌

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp1/test1.jsp</title>
</head>
<body>
    <!-- http://localhost:8080/JspProject/jsp1/test1.jsp -->
    <h1>jsp1/test1.jsp</h1>
     <!-- 예제1 -->
    <form action = testPro1.jsp method = "get">
        
        <!-- 아이디 -->
        <div>
            <label for = id> 아이디 : </label>
            <input type = "text" name = "id">
        </div>
        
        <!-- 좋아하는 숫자 -->
        <div>
            <label for = num> 좋아하는 숫자 :  </label>
            <input type = "text" name = "num">
        </div>
        
        <!-- 전송버튼 -->
        <div>
            <input type = "submit" name = "button" value = "전송">
        </div>
    </form>

</body>
</html>
```

- 예)JSP get 명령으로 test1에서 사용자가 입력한 값 을 표시함.

```HTML
<div>
	<%=request.getParameter("id")%>
</div>
<div>
	<%=request.getParameter("num")%>
</div>
```

- ex2)

```HTML
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp/test2.jsp</title>
</head>
<body>
    <h1>jsp/test2.jsp</h1>
	<form action = testPro2.jsp method = "get">
	
	<!-- 아이디 -->
	<div>
		<label for = id> 아이디 : </label>
		<input type = "text" name = "id">
	</div>
	
	<!-- 좋아하는 숫자 -->
	<div>
		<label for = num> 좋아하는 숫자 : </label>
		<input type = "text" name = "num">
	</div>
	
	<!-- 라디오 버튼 -->
	<div>
		<label for = gender> 성별 : </label>
		<input type = "radio" name = "gender" value = "남"> 남
		<input type = "radio" name = "gender" value = "여"> 여
	</div>
	
	<!-- 전송버튼 -->
	<div>
	   <input type = "submit" name = "button" value = "전송">
	</div>
	</form>

</body>
</html>
====================================================================================

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp1/testPro2.jsp</title>
</head>
<body>
    <h1>jsp1/testPro2.jsp</h1>
    <%
    //자바, jsp명령 여러줄
    %>
    <div>
        이름 : <%=request.getParameter("id")%>
    </div>
    <div>
        나이 : <%=request.getParameter("num")%>
    </div>
    <div>
        성별 : <%=request.getParameter("gender")%>
    </div>

</body>
</html>
```

![[Untitled 4 4.png|Untitled 4 4.png]]

- 값 입력하고 전송을 눌러본다!

![[Untitled 5 4.png|Untitled 5 4.png]]

- 사용자가 입력한 값을 받아 올 수 있다!
- <%@ %> : 페이지 설정
- <% %> : 자바, jsp 명령
- <%= %> : 값, 변수 ==하나== 출력!
- jstl, el : 태그처럼 출력
