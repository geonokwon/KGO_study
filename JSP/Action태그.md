# Action태그
### Include
- 액션태그 : jsp문법, 태그처럼 사용하게 만든 문법
- jsp:include 액션태그 : 반복되는 메뉴을 파일로 분리하고 파일을 가져와서 (include) 사용 -> 수정을 최소화
```HTML title:HTML(include.jsp)
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp3/include.jsp</title>
</head>
<body>
<h3>jsp3/include.jsp</h3>

<%-- <jsp:include page="반복되는 내용을 파일로 만들어서 불러옴"></jsp:include> --%>
	<table border = "1">
		<tr>
			<td colspan = "2" width = "600" height = "100">
			<jsp:include page="top.jsp"/>
			</td>
		</tr>
		<tr>
			<td height = "500">왼쪽메뉴</td>
			<td>본문1</td>
		</tr>
		<tr>
			<td colspan = "2" width = "600" height = "100">
				<jsp:include page="bottom.jsp"/>
			</td>
		</tr>
	</table>
</body>
</html>
```

```HTML title:HTML(include2.jsp)
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp3/include.jsp</title>
</head>
<body>
<h3>jsp3/include.jsp</h3>

<%-- <jsp:include page="반복되는 내용을 파일로 만들어서 불러옴"></jsp:include> --%>
	<table border = "1">
		<tr>
			<td colspan = "2" width = "600" height = "100">
			<jsp:include page="top.jsp"/>
			</td>
		</tr>
		<tr>
			<td height = "500">왼쪽메뉴</td>
			<td>본문2</td>
		</tr>
		<tr>
			<td colspan = "2" width = "600" height = "100">
				<jsp:include page="bottom.jsp"/>
			</td>
		</tr>
	</table>
</body>
</html>
```

```HTML title:HTML(include3.jsp)
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp3/include.jsp</title>
</head>
<body>
<h3>jsp3/include.jsp</h3>

<%-- <jsp:include page="반복되는 내용을 파일로 만들어서 불러옴"></jsp:include> --%>
	<table border = "1">
		<tr>
			<td colspan = "2" width = "600" height = "100">
			<jsp:include page="top.jsp"/>
			</td>
		</tr>
		<tr>
			<td height = "500">왼쪽메뉴</td>
			<td>본문3</td>
		</tr>
		<tr>
			<td colspan = "2" width = "600" height = "100">
				<jsp:include page="bottom.jsp"/>
			</td>
		</tr>
	</table>
</body>
</html>
```

```html title:HTML(top.jsp)
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>

<h3>jsp3/top.jsp</h3>
<a href = 'include.jsp'>메뉴1</a>
<a href = 'include2.jsp'>메뉴2</a>
<a href = 'include3.jsp'>메뉴3</a>
<a href = 'include4.jsp'>메뉴4</a>
<a href = 'include5.jsp'>메뉴5</a>
```

```html title:HTML(bottom.jsp)
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<h3>jsp3/bottom.jsp</h3>
<p>주소 : 부산광역시 부산진구 동천로 109 삼한골든게이트빌딩 7층</p>
<p>TEL : 051-803-0000</p>
<p>이메일 : class@itwillbs.co.kr</p>
```

### 쿠키(Cookie)
- 세션 : 공통점 - 연결정보를 저장, 페이지 상관없이 값을 유지 
  차이점 - 서버에 저장(서버에 메모리에 저장), 로그인정보 저장
- 쿠키 : 공통점 - 값을 저장, 페이지 상관없이 값을 유지 
  차이점 - 클라이언트에 저장(사용자 컴퓨터에 하드웨어 파일로 저장), 서버에 부하를 줄이면서 보안상에 상관없는 내용을 저장
   => 서버종료, 연결종료, 사용자 컴퓨터 종료 상관없이 값을 유지하고 싶을때 사용
   => 쿠키 자바로 만든 파일(서버가 만들지 않고 개발자가 만들어서 사용)

#### 쿠키 값 만들어 보기
- cookie 기억장소를 만들고 이름, 값을 저장
```html title:HTML
Cookie cookie = new Cookie("cName", "cookieValue");
out.println("cookie 기억장소 주소 : " + cookie + "<br>");
out.println("cookie 이름 : " + cookie.getName() + "<br>");
out.println("cookie 이름에 해당하는 값 : " + cookie.getValue() + "<br>");
```

- cookie 시간 설정(초) => 시간 설정 한 만큼 유지
```html title:HTML
cookie.setMaxAge(60*60);
out.println("cookie 유지시간 : " + cookie.getMaxAge() + "<br>");
```

- 서버에서 => 사용자에게 전달 => reponse 저장 => 웹서버 => http => 사용자 컴퓨터 하드웨어에 파일로 저장
```html title:HTML
response.addCookie(cookie);
```

#### 쿠키 값 가져오기
- 사용자가 요청하면 http(서버정보, 사용자정보, 쿠키정보, 세션정보, http헤더정보, 태그정보) 들고
-  => 웹서버에 도착 => 웹애플리케이션 서버 도착 =>
-  => 1)request, response 기억장소 만들고, request 에 요청정보저장
-  => 2)web.xml 참조
-  => 3)서블릿 처리 작업
  

- request 에서 쿠키정보 가져오기
- Cookie 여러개 저장할수 있는 변수(배열) = request에서 가져온 어러개값을 저장

- 쿠키값이 null 일때
```html title:HTML
if(cookies == null){
	out.println("쿠키가 없습니다.");
}
else {
	out.println("쿠키주소 : " + cookies + "<br>");
	out.println("쿠키배열 주소 기억장소 크기 : " + cookies.length + "<br>");
}
```

```html title:JSP
<%
for (int i = 0; i < cookies.length; i++){
//여러개 쿠기값 중에 쿠키 이름을 찾기 "cName" 찾기
	if(cookies[i].getName().equals("cName")){
	%>
		<h4> 일치하는 쿠키값을 가져왔습니다.</h4>
		<h4><%=i %> 쿠키의 주소 : <%=cookies[i] %></h4>
		<h4><%=i %> 쿠키의 이름 : <%=cookies[i].getName() %></h4>
		<h4><%=i %> 쿠키의 이름 : <%=cookies[i].getValue() %></h4>
	<%
	break;
	}
	else {
		%>
		<h4>쿠키를 찾지 못했습니다.</h4>
		<%
	}
}
%>
```


#### 쿠키 값 삭제하기
- 서버중지, 브라우저 종료, 사용자 컴퓨터 종료해도 쿠키이름, 값 삭제 되지 않음
	- http통해서 쿠키값을 가져와서 서버 request에 쿠키값이 저장
	- request에서 쿠키값을 가져와서 => 배열변수 담기 => for 모두 배열에 담긴 쿠키 접근
	- 쿠키값이 null이 아니면
	- if 쿠키이름 찾아서 => 찾은 쿠키의 시간을 0으로 설정
	- response에 0으로 설정된 쿠키를 저장 => 사용자 컴퓨터에 전달 => 쿠키값 삭제
	- 쿠키이름 "cName" 쿠키값 "cookieValue" => 삭제

```html title:JSP
<%
Cookie[] cookies = request.getCookies();
//쿠키값이 null이 아닐경우에만!
if(cookies != null){
	for(int i = 0; i < cookies.length; i++){
		//지우고자 하는 쿠키값의 네임과 일치하는지 판단
		if(cookies[i].getName().equals("cName")){
			cookies[i].setMaxAge(0); //일치하는 쿠키값의 유지시간을 0으로 만든다.
			response.addCookie(cookies[i]); //0으로만든 쿠키값을 다시 보낸다.
			response.sendRedirect("cookie.jsp"); //0으로만들고 보낸후 다시 원래 페이지로 돌아간다.
			break; //일치하는 값을 찾았으므로 더이상 포문을 돌필요없어서 빠져나간다.
		}
	}
	%>
		<!-- 값을 찾지 못하면 다시 처음으로 돌아가는 링크 주기! -->
		<h4>값을 찾지 못했습니다.</h4>
		<a href = "cookie.jsp">돌아가기</a>
	<%
}
%>


```

#### 쿠키 사용해보기
- http가 사용자가 선택한 값을 서버에 전달 => 서버에 request 기억장소 (내장객체)에 저장
-  request에서 태그 정보를 가져와서 => 변수에 저장
- radio 버튼에서 language 선택시 -> kroea 벨류 반환
- \http://localhost:8080/JspProject/jsp3/cookieExample1.jsp?language=korea
- radio 버튼에서 language 선택시 -> english 벨류 반환
- /http://localhost:8080/JspProject/jsp3/cookieExample1.jsp?language=english

```html title:JSP(cookieExample1.jsp)
<%
//response로 받아온 쿠키값 배열에 저장
Cookie[] cookie = request.getCookies();
   //배열로 쿠키값 순회한다
for(int i = 0; i < cookie.length; i++){
	//배열의 순회하면서 벨류값이 쿠키의 벨류값이 korea 인지 아닌지 판단
	if(cookie[i].getValue().equals("korea")){
		%>
		// valus 값이 korea이면 한글 표시
		<h3>안녕하세요 쿠키 예제입니다.</h3>
		<%
	}
	//배열의 순회하면서 벨류값이 쿠키의 벨류값이 english 인지 아닌지 판단
	if(cookie[i].getValue().equals("english")){
		%>
		// valus 값이 english이면 영어 표시
		<h3>hello. This is Cookie Example.</h3>
		<%
	}
}
%>
<form action = "cookieExample2.jsp" method = "get">
	<input type = "radio" name = "language" value = "korea">한국어 페이지 보기
	<input type = "radio" name = "language" value = "english">영어 페이지 보기
	<input type = "submit" value = "설정">
</form>
``` 

```html title:JSP(cookieExample2.jsp)
//request로 받아온 language의 value값 받아와 변수에 저장
String language = request.getParameter("language");
out.println(language);
//변수에 저장한 value 값을 네임,벨류 값으로 쿠키 생성
Cookie cookie = new Cookie("language", language);
cookie.setMaxAge(60 * 60);
//생성한 쿠키 response로 반환
response.addCookie(cookie);
//반환후 다시 원래 페이지로 돌아가기
response.sendRedirect("cookieExample1.jsp");
```

