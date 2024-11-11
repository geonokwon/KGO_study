- 로그인 창
```html title:JSP
<!-- SQL 삽입 공격
아이디 : kim' OR '1'='1
비밀번호 틀리게 입력 => 로그인 됨
-->
<form action = "LoginPro2.jsp" method = "post">
	<div><p>로그인</p></div>
	<!-- 아이디 -->
	<div>
		<input type = "text" name = "id" placeholder = "아이디">
	</div>
	<!-- 비밀번호 -->
	<div>
		<input type = "password" name = "pass" placeholder = "비밀번호">
	</div>
	<!-- 전송버튼(input type = submit) -->
	<div class = "button">
		<input type = "submit" value = "로그인">
	</div>
	<div class = "button">
		<a href = "Insert.jsp">회원가입 하기</a>
	</div>
</form>
```

- Statement -> 보완에 취약한 약점을 가지고 있음.
```html title:JSP
//SQL 삽입공격시 true로 나오며 next(); 조건문에 트루로 동작하게 됨!

String sql_Select = "SELECT * FROM members WHERE id = '" + id + "' AND pass = '" + pass + "'";
Statement select = con.createStatement();
ResultSet result_select = select.executeQuery(sql_Select);
```