### Request 사용법
- http가 사용자가 입력한 정보를 서버에 전달 => 서버request 내장객체(기억장소)에 저장
- request에서 값을 가져와서 => 변수에 저장

```html title:(test3.jsp)

<!-- test3.jsp -->

<!-- 회원가입 form-->
<form action = "testPro3.jsp" method = "post">
<!-- 아이디 입력란 -->
<div>
	<label for = "id"> 아이디 </label>
	<input type = "text" name = "id" value="아이디를 입력">
</div>

<!-- 비밀번호 입력란 -->
<div>
	<label for = "password">비빌번호</label>
	<input type = "password" name = "password">
</div>

<!-- 자기소개 -->
<div>
	<label for = "intro">자기소개</label>
	<textarea rows = "10" cols = "20" name = "intro"></textarea>
	<!-- value 값 없음 -->
</div>

<!-- 성별 -->
<div>
	<label for = "gender">성별</label>
	<input type = "radio" name = "gender" value = "남">남성
	<input type = "radio" name = "gender" value = "여">여성
	<!-- name 이 동일하면 radio 태그는 그룹으로 인식하고 => 그룹중에 하나의 선택 -->
</div>

<!-- 취미 -->
<div>
	<label for = "hobby">취미</label>
	<input type = "checkbox" name = "hobby" value = "축구">축구
	<input type = "checkbox" name = "hobby" value = "게임">게임
	<input type = "checkbox" name = "hobby" value = "야구">야구
	<!-- name 이 동일하면 checkbox 태그는 그룹으로 인식하고 => 그룹중 여러개 선택 -->
</div>

<!-- 목록 -->
<div>
	<label for = "grade">등급(목록)</label>
	<select name = "grade">
		<option value = "목록1">목록1</option>
		<option value = "목록2">목록2</option>
		<option value = "목록3">목록3</option>
	</select>
	<!-- 여러개 선택 multiple << 많이는 사용하지 않음! -->
</div>

<!-- 숨김값 서버 전달 -->
<div>
	<label for = "hidden">숨긴값 서버 전달</label>
	<input type = "hidden" name = "hidden" value = "hidvalue">
	<!-- 화면에는 보이지 않지만 값이 서버로 전달된다 -->
</div>

<!-- 전송버튼-->
<div>
	<input type = "submit" value = "가입(전송,제출)">
</div>

<!-- form(하이퍼 링크)태그,양식태그 - 사용자의 값을 들고(id=aaaa&pass=123) 서버로 요청하러 간다-->

<!-- 전송버튼 눌렀을시 test1.html로 이동 하면서 아이디,비밀번호 값을 같이 들고감!-->

<!-- http://localhost:8080/JspProject/html/test1.html?id=aaaa&pass=123 -->
</form>
```

- test3.jsp 에서 값을 가져와 testPro3에서 보여준다!
```html title:(testPro3.jsp)
<!-- testPro3.jsp -->

<%
String id = request.getParameter("id");
int pawssword = Integer.parseInt(request.getParameter("password"));
String intro = request.getParameter("intro");
String gender = request.getParameter("gender");
String hobby = request.getParameter("hobby");
String grade = request.getParameter("grade");
String hidden = request.getParameter("hidden");
%>
<div>
	아이디 : <%=id %>
</div>

<div>
	비밀번호 : <%=pawssword %>
</div>

<div>
	자기소개 : <%=intro %>
</div>

<div>
	성별 : <%=gender %>
</div>

<div>
	취미 : <%=hobby %>
</div>

<div>
	등급 : <%=grade %>
</div>

<div>
	숨김값 : <%=hidden %>
</div>
```

### Request(쿠키,URL,URI, ...) 요청방식과 관련된 메소드
- http가 요청하러 서버에 갈때 (태그정보, 서버정보,사용자(클라이언트)정보, 세션정보, 쿠키정보, http더정보) 정보를 들고 서버에 전달 => 서버 request 내장객체(기억장소)에 저장
```html title:(request.jsp)
<h1>jsp1/request.jsp</h1>

<%
// http가 요청하러 서버에 갈때 (태그정보, 서버정보,사용자(클라이언트)정보, 세션정보, 쿠키정보, http해더정보)
// 정보를 들고 서버에 전달 => 서버 request 내장객체(기억장소)에 저장
%>

<div>
태그정보 : <br>
서버 이름 : <%=request.getServerName() %><br>
서버 포트 : <%=request.getServerPort() %><br>
요청 URL(요청 주소) : <%=request.getRequestURL() %><br>
요청 URI(서버 뺀 나머지 주소) :<%=request.getRequestURI() %><br>
요청 프로젝트 : <%=request.getContextPath() %><br>
요청 (프로젝트 뺀 나머지 주소) : <%=request.getServletPath() %>
http 프로토콜(통신규약) 정보 : <%=request.getProtocol() %><br>

데이터 전송 방식 : <%=request.getMethod() %><br>

사용자 ip주소 : <%=request.getRemoteAddr() %><br>

http 헤더정보 ("accept-language") : <%=request.getHeader("accept-language") %><br>
http 헤더정보 ("user-agent") : <%=request.getHeader("user-agent") %><br>
http 헤더정보 ("host") : <%=request.getHeader("host") %><br>
</div>
```

### Response 
- http가 요청하러 오면 서버 => request(요청정보저장), response(응답정보저장) 기억장소가 만들어 지면서
- => request에 요청값 저장
- => 처리담당자 처리하면서 => 결과 response 응답정보값 저장
- => response 를 웹서버에 전달 => response를 http에 전달
- => http가 사용자 전달
- => request, response, 처리담당자(sevelet) 기억장소 해제, 연결종료

- response => 서버가 사용자에게 설정을 변경할때 사용

- 사용자의 http header 정보 변경 명령
```HTML title:(response.jsp)
<%
response.setHeader("accept-language", "ko-KR");
%>
```

- 사용자의 내용타입 변경
```HTML title:(response.jsp)
<%
response.setContentType("text/html; charset=UTF-8");
%>
```

- 사용자의 컴퓨터에 쿠키값을 저장하는 명령
```HTML title:(response.jsp)
<%
response.addCookie(쿠키);
%>
```

- 사용자에게 다른 페이지로 이동하게 만드는 명령(하이퍼링크)
```HTML title:(response.jsp)
<%
response.sendRedirect("request.jsp");
%>
```



