-> MySQL 환경변수 설정( SETX PATH “C:\\Program Files\\MySQL\\MySQL Server 8.0\\bin;%PATH%”)
-> JDBC 설치(자바와 데이터 베이스 연결 => DB에서 제공) - connector/J
-> mysql-connector-java-8.0.17.jar

1. JDBC 프로그램 가져오기
	-> project -> src -> main ->webapp -> WEB_INF -> lib -> 위치
	- com/mysql/cj/jdbc/Driver.class 파일을 불러오기
	- C:\\Program Files\\ojdkbuild\\java-11-openjdk-11.0.15-1\\lib
	- src\\java.base\\java\\lang 폴더 -> Class.java 자바파일
	- java 프로그램 (JDK 설치 할떄 => 자동으로 설치)
	- java폴더/lang폴더 -> Class.java(Class.Class) => Java 프로그램 사용 => 기본폴더
	- Class 파일 관리하는 명령모음

```
Class.forName("com.mysql.cj.jdbc.Driver")
```

2. JDBC 프로그램 이용해서 데이터베이스 접속
	- java폴더 sql폴더 ->DriverManager 자바프로그램 -> JDBC driver 관리하는 프로그램
	- import = "java.sql.DriverManager" 자바프로그램 위치 표시.
	- .getConnection(url, user, password) -> 데이터베이스 연결 명령
	- url : 데이터베이스 서버 웹 주소, user : 데이터베이스 서버에 접속할 수 있는 user, password : 데이터베이스 서버 접속 비밀번호

	- import="java.sql.Connection 자바프로그램 위치
	- Connection : 데이터베이스 연결 정보를 저장 프로그램
	-  url => ?serverTimezone=Asia/Seoul 추가해서 타임존 오류 해결
```
String dburl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
String dbuser = "root";
String dbpassowrd = "1234";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);
```


3. SQL구문을 만들어서 실행할 준비.
4. SQL구문을 실행 (insert, update, delete)
	- SQL 구문 실행시 결과값 조회 (select)
5. 결과조회 -> 출력, 기억장소 저장 -> select 만 쓴다.

- ### INSERT 구문 적용
```java title:JSP
<h2>jsp4/InsertPro.jsp</h2>

<%
// 사용자가 입력한 학생번호 와 학생이름을 => 서버에 전달 => request 기억장소 => 저장
// request에 저장된 num, name 을 가져와서 => 변수에 저장

// int num = Integer.parseInt(request.getParameter("num"));
String num = request.getParameter("num");
String name = request.getParameter("name");


%>
<p>학생번호 : <%=num %> </p>
<p>학생이름 : <%=name %> </p>
<%

// 1단계 JDBC 프로그램 가져오기
Class.forName("com.mysql.cj.jdbc.Driver");

// 2단계 JDBC 프로그램 이용해서 데이터베이스 접속
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
String dbUser = "root";
String dbPass = "1234";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

// 3단계 SQL구문을 만들어서 실행할 준비.
// String sql = "INSERT INTO student(num,name) values(2,'김길동')";
// String sql = "INSERT INTO student(num,name) values(" + num +",'" + name + "')";
String sql = "INSERT INTO student(num,name) values(?, ?)";
PreparedStatement pstmt = con.prepareStatement(sql);

// 4단계 SQL구문을 실행 (insert, update, delete)
pstmt.executeUpdate();
```

- ### UPDATE 적용
```java title:JSAP

<h3>jsp4/updatePro.jsp</h3>
<%
//사용자가 수정할 내용을 가져오면 => 서버에 전달 => request 기억장소 저장
// request에서 학생번호 num, 수정할 학생이름 name 가져와서 => 변수에 저장
Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
String dbUser = "root";
String dbPass = "1234";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

int num = Integer.parseInt(request.getParameter("num"));
String name = request.getParameter("name");
String sql_update = "UPDATE student SET name = ? WHERE num = ?";
PreparedStatement update = con.prepareStatement(sql_update);
update.setString(1, name);
update.setInt(2, num);
update.executeUpdate();
%>
<p>회원번호 : <%=num %></p>
<p>변경된 회원이름 : <%=name %></p>
<%
out.println("회원이름이 변경되었습니다.");
%>
```

- ### DELETE 적용
```java title:JSP
<h3>jsp4/deletePro.jsp</h3>

<%
Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
String dbUser = "root";
String dbPass = "1234";

Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);
int num = Integer.parseInt( request.getParameter("num"));
String name = request.getParameter("name");
String sql_delete = "DELETE FROM student WHERE num = ? AND NAME = ?";
PreparedStatement delete = con.prepareStatement(sql_delete);
delete.setInt(1, num);
delete.setString(2, name);
delete.executeUpdate();
%>

```

- ### SELECT 적용
- 결과에서 행을 접근 next() 다음행 -> 데이터가 있으면 결과 true, 데이터가 없으면 false 반환
```java title:JSP
<%
Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimezone=Asia/Seoul";
String dbUser = "root";
String dbPass = "1234";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

String sql_select = "SELECT * FROM student";
PreparedStatement select = con.prepareStatement(sql_select);
ResultSet rs = select.executeQuery();

while(rs.next()){
	int num = rs.getInt("num");
	String name = rs.getString("name");
	%>
	<p><%=num %></p>
	<p><%=name %></p>
	<%
}

%>
```




------
--------------------------------
- 혼자 연습으로 만들어본것 DB연동
- (Insert.jsp)
```
<h2>jsp1/Insert.jsp</h2>
<h3>추가할 회원정보</h3>
<form action = "InsertPro.jsp" method = "get">
	<div>
		<label for = "num_Add">회원번호 : </label>
		<input type = "text" name = "num_Add" id = "num">
	</div>
	<div>
		<label for = "name_Add">회원이름 : </label>
		<input type = "text" name = "name_Add" id = "name">
	</div>
	<div>
		<input type = "submit" value = "전송!">
	</div>
</form>
<hr>

<h3>삭제할 회원정보</h3>
<form action = "InsertProDelete.jsp" method = "get">
	<div>
		<label for = "num_Del">회원번호 : </label>
		<input type = "text" name = "num_Del" id = "num_Del">
	</div>
	<div>
		<label for = "name_Del">회원이름 : </label>
		<input type = "text" name = "name_Del" id = "name_Del">
	</div>
	<div>
		<input type = "submit" value = "삭제!">
	</div>
</form>
<hr>

<h3>회원정보 전체삭제!</h3>
<form action = "InsertDeleteAll.jsp" method = "get">
	<button type = "submit" name = "delete" value = "delete">전체삭제!!</button>
</form>
<%

Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimeZone=Asia/seoul";
String dbUser = "root";
String dbPass = "12345678";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

String sql_Select = "SELECT * FROM student";
PreparedStatement select = con.prepareStatement(sql_Select);
ResultSet rs = select.executeQuery();
int result_num;

String result_name;
while(rs.next()){
	result_num = rs.getInt("num");
	result_name = rs.getString("name")
	%>
		<p>회원번호 : <%=result_num %></p>
		<p>회원이름 : <%=result_name %></p>
	<%
}
%>
```

- (InsertPro)
```
<h2>jsp1/InsertPro.jsp</h2>

<%
Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimeZone=Asia/seoul";
String dbUser = "root";
String dbPass = "12345678";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

int num_Add;
String name_Add;
if(!request.getParameter("num_Add").isEmpty() && !request.getParameter("name_Add").isEmpty()){
	try{
		num_Add = Integer.parseInt(request.getParameter("num_Add"));
		name_Add = request.getParameter("name_Add");
		String sql_Insert = "INSERT INTO student(num, name) values(?, ?)";
		PreparedStatement insert = con.prepareStatement(sql_Insert);
		insert.setInt(1, num_Add);
		insert.setString(2, name_Add);
		insert.executeUpdate();
		out.println("학생데이터 입 완료!!<br>");
		response.sendRedirect("Insert.jsp");
	}
	catch (Exception e){
		e.printStackTrace();
		out.println("추가중 오류가 발생했습니다.");
	}
}
%>
<a href = "Insert.jsp">이전으로 돌아가</a>
```

- (InsertProDelete)
```
<h3>jsp1/InsertProDelete.jsp</h3>

<%
Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimeZone=Asia/seoul";
String dbUser = "root";
String dbPass = "12345678";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

int num_Del;
String name_Del;
if(!request.getParameter("num_Del").isEmpty() && !request.getParameter("name_Del").isEmpty()){
	try{
		num_Del = Integer.parseInt(request.getParameter("num_Del"));
		name_Del = request.getParameter("name_Del");
		out.println(num_Del);
		String sql_delete = "DELETE FROM student WHERE num = ?";
		PreparedStatement delete = con.prepareStatement(sql_delete);
		delete.setInt(1, num_Del);
		delete.executeUpdate();
		out.println(name_Del + " 님의 데이터가삭제되었습니다.");
		response.sendRedirect("Insert.jsp");
	}
	catch (Exception e){
		e.printStackTrace();
		out.println("삭제중 오류가 발생했습니다.");
	}
}

%>
<a href = "Insert.jsp">이전으로 돌아가</a>
```

- (InsertDeleteAll)
```
<h3>jsp1/InsertDeleteAll.jsp</h3>

<%
Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimeZone=Asia/seoul";
String dbUser = "root";
String dbPass = "12345678";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

String delete = request.getParameter("delete");
if(delete.equals("delete")){
	try {
		String sql_delete = "DELETE FROM student";
		PreparedStatement delete_All = con.prepareStatement(sql_delete);
		delete_All.executeUpdate();
		out.println("학생데이터 전체삭제 완료!!<br>");
		response.sendRedirect("Insert.jsp");
	}
	catch (Exception e) {
		e.printStackTrace();
		out.println("삭제중 오류 발생<br>");
	}
}
%>

<a href = "Insert.jsp">이전으로 돌아가</a>
```

- (selectLogin.jsp)
```
<h3>jsp1/selectLogin.jsp</h3>

<%

Class.forName("com.mysql.cj.jdbc.Driver");
String dbUrl = "jdbc:mysql://localhost:3306/jspdb?serverTimeZone=Asia/seoul";
String dbUser = "root";
String dbPass = "12345678";
Connection con = DriverManager.getConnection(dbUrl, dbUser, dbPass);

String user_name = request.getParameter("name_Login");
String sql_Select = "SELECT name FROM student WHERE name = ? ";
PreparedStatement select = con.prepareStatement(sql_Select);
select.setString(1, user_name);
ResultSet result_name = select.executeQuery();
result_name.next(); //select구문처럼 조회해서 받아오는 ResultSet으로 받아온 값은 next()로 포인터를 이동시켜줘야함.

String name = result_name.getString("name");
if(name.equals(user_name)){
	%>
	<h4>로그인성공!!</h4>
	<p>회원이름 : <%=name %></p>
	<%
}

out.println(result_name);
out.println(name);
%>
```