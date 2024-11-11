#### JSP, JSTL
##### member
- Login
```html title:Login
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/Login.jsp</title>
<style>
    
    .title {
        text-align: center;
        padding-top: 10px;
        margin-top: 100px;
        font-size: 30px;
        color : gray;
    }
    p {
        margin-bottom: 0px
    }
    .background_page{
        width: 400px;
        height: 320px;
        border: 2px solid #dadada;
        margin: 10px auto;
        border-radius: 20px;
    }
    form{
        text-align: center;
        padding-top: 38px
    }
    form div {
        padding : 10px;
        padding-bottom: 0px;
    }
    input {
        width: 206px;
        height: 42px;
        padding:0px;
        margin: 4px 36px;
        border: 1px solid #dadada;
        border-radius: 10px;
        padding: 0px 8px;
        
    }
    
    .id_pass input{
        text-align: left;
    }
    .button {
        padding-top: 20px;
    }
    button:hover {
        background-color: #dadada;
    }
    .button button{
        text-align: center;
        font-size: 14px;
        border: 1px solid #dadada;
        border-radius: 10px;
        width: 224px;
        height: 44px;       
    }
    .membership{
        padding-top: 30px;
    }
    a {
        color: gray;    
    }
    a:hover {
        color: #dadada;
    }
    .Login_check p{
        text-align: center;
    }
</style>
</head>
<body>
    <div class = "title" onclick="location.href = 'login.me' ">
        <p>LOGIN</p>
    </div>
    <%
    session.invalidate();
    %>
    <div class = "background_page">
        <form action = "loginPro.me" method = "post">
            <div class = "id_pass">
                <!-- 아이디 -->
                <div>
                    <input type = "text" name = "id" id = "id" placeholder = "아이디">
                </div>
                <!-- 비밀번호-->
                <div>
                    <input type = "password" name = "pass" id = "password" placeholder = "비밀번호">
                </div>
            </div>
            <!-- 로그인 버튼 -->
            <div class = "button">
                <button type = "submit">로 그 인</button>
            </div>
            <div class = "membership">
                <a href = "insert.me" >회원가입 하시겠습니까?</a>
            </div>
        </form>
    </div>

</body>
</html>
```

- Insert(회원가입)
```HTML title:Insert
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/Insert.jsp</title>
<style>
        .title {
            text-align: center;
            padding-top: 10px;
            margin-top: 100px;
            font-size: 20px;
            color: gray;
        }

        .title p {
            margin-bottom: 2px;
        }

        .background {
            width: 400px;
            height: 320px;
            margin: 10px auto;
            padding: 10px;
            text-align: center;
            /* border: 2px solid #dadada; */
            /* border-radius: 20px; */
        }

        input {
            width: 300px;
            height: 42px;
            padding: 0px;
            margin: 4px 36px;
            border: 1px solid #dadada;
            border-radius: 10px;
            padding: 0px 8px;
            color: gray;
        }
        
        button {
            width: 318px;
            height: 42px;
            margin: 3px;
            border: 1px solid #dadada;
            border-radius: 10px;
        }
       
        .gender {
            align-content: center;
            width: 318px;
            height: 42px;
            padding: 0px;
            margin: 4px 40px;
            border: 1px solid #dadada;
            border-radius: 10px;
            
		}
		.gender input[type=radio]{
		    display: none;
		}
		.gender input[type=radio]+label{
		    display: inline-block;
		    cursor: pointer;
		    height: 24px;
		    width: 90px;
		    border: 1px solid #dadada;
		    border-radius: 8px;
		    line-height: 24px;
		    text-align: center;
		    font-weight:bold;
		    font-size:13px;
		}
		.gender input[type=radio]+label{
		    background-color: #fff;
		    color: #dadada;
		}
		.gender input[type=radio]:checked+label{
		    background-color: gray;
		    color: #fff;
		}

        .isLogin_true {
            font-size: 16px;
            color: green;
        }
        .isLogin_false {
            font-size: 16px;
            color: red;
        }
        .error{        
            border: 1px solid red;
        }
    </style>
</head>
<body>
    <div class="title">
        <p>회원가입</p>
    </div>

    <div class="background">
        <form action="insertPro.me" method="post">
            <!-- 아이디-->
            <div>
                <input type="text" name="id" placeholder="아이디" class="input_check" onblur="checkInput(this)" maxlength = "10" required>
            </div>
            <!-- 비밀번호-->
            <div>
                <input type="password" name="pass" placeholder="비밀번호" class="input_check" maxlength = "15" onblur="checkInput(this)" required>
            </div>
            <!-- 이름 -->
            <div>
                <input type="text" name="name" placeholder="이름" class="input_check" maxlength = "5" onblur="checkInput(this)" required>
            </div>
            <!-- 회원가입 버튼 -->
            <div>
                <button type="submit">회원가입</button>
            </div>
        </form>
        </div>
    <script>
    //숫자 외에 다른 문자가 들어오면 클리어함! 숫자만 쓸수 있게 만듬!
    function checknumber(input) {
        // 숫자와 소수점을 제외한 모든 문자 제거
        let clear_Value = input.value.replace(/[^0-9.]/g, '');
        // 여러 개의 소수점 중 첫 번째 소수점만 남김
        clear_Value = clear_Value.replace(/(\..*)\./g, '$1');
        // 입력 값 업데이트
        input.value = clear_Value;
    }
    
   //input 테그에 입력문자가 없다면 빨간색으로 표시하게 하는 함수!
     function checkInput(input) {
         if (input.value.trim() === "") {
             input.classList.add("error");
         } else {
             input.classList.remove("error");
         }
     }
    </script>
</body>
</html>
```

- Main(메인화면)
```html title:Main
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html lang = "ko">
<head>
<meta charset="UTF-8">
<meta http-equiv="Refresh" content="86400">
<title>member/Main.jsp</title>
<style>
    @import url('https://fonts.googleapis.com/css2?family=Jua&display=swap');
    
    * {
        color: gray;
        font-family: "Jua", sans-serif;
        font-weight: 400;
        font-style: normal;;
    }
    
    h2 {
        font-size: 40px;
        margin-bottom: 10px;
    }
    .background {
        width: 160px;
        height: 1000px;
        text-align: center;
        border: 1px solid #dadada;
        border-radius: 20px;
    }
    .left-Sidebar {
        font-size: 18px;
	    text-align: center;
	    margin-top: 10px;
    }
    .left-Sidebar button{
        width:120px;
        font-size: 16px;
        border: 1px solid #dadada;
        border-radius: 6px;
        margin-bottom: 10px;
    }
    .left-Sidebar button:hover{
        background-color: #dadada;
    }
    .title {
        margin: 10px 0px 10px 0px;
    }
    .Main {
        display: flax;
        width: 160px;
        height: 1000px;
        text-align: center;
        border: 1px solid #dadada;
        border-radius: 20px;
    }
</style>
</head>
<body>
    <h2>Main Page</h2>
    <div class = "background">
        <c:if test="${!empty sessionScope.id}">
	    	<div class = "left-Sidebar">
			    <div class = title>Name : ${sessionScope.id}</div>
			    <button type = "button" onclick="location.href = 'write.bo'">글쓰기</button>
			    <button type = "button" onclick="location.href = 'list.bo?pageNum=1'">게시판</button>
			    <button type = "button" onclick="location.href = 'info.me'">나의정보</button>
			    <button type = "button" onclick="location.href = 'logout.me'">로그아웃</button>
	    	    <c:if test="${sessionScope.id == 'root'}">
	    	      <button type = "button" onclick="location.href = 'memberList.me'">회원목록</button>
	    	    </c:if>
	    	</div>
        </c:if>
        <c:if test="${empty sessionScope.id}">
		    <div class = "left-Sidebar">
		        <button type = "button" onclick="location.href = 'login.me'">로그인</button>
	        </div>
        </c:if>
    </div>
</body>
</html>
```

- Info(나의정보 페이지)
```html title:Info
<%@page import="com.itwillbs.domain.MemberDTO"%>
<%@page import="java.sql.Timestamp"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/Info.jsp</title>
<style>
    .background {
        text-align: center;
        color: gray;
        margin: 30px auto;
        padding-left: 20px;
        width: 300px;
	    height: auto;
	    border: 1px solid #dadada;
	    border-radius: 20px;
    }
    .in {
        display: flex;
        align-items: center;
        margin-top: 10px;
        margin-bottom: 10px;
    }
    .in label{
        width:88px;
        display: inline-block;
        text-align: left;
    }
    .background lebel{
        margin-right: 20px;
    }
    .background input {
        text-align: center;
        padding-left: 10px;
        color: gray;
        height: 40px;
        border: 1px solid #dadada;
        border-radius: 10px;
    }
    .background p{
        text-align: center;
        font-size: 20px;
        font-weight: bold;
    }
    button{
        margin-top: 5px;
        margin-bottom: 10px;
        color: gray;
        width: 200px;
        height: 30px;
        border: 1px solid #dadada;
        border-radius: 10px;
    }
</style>
</head>
<body>
    <c:choose>
        <c:when test="${!empty requestScope.dto}">
		    <div class = "background">
		        <p>회원정보</p>
		        <div class = "in">
		            <label>아이디 : </label>
		            <input type = "text" name = "id" value = "${dto.id}" readonly>
		        </div>
		        <div class = "in">
		            <label>비밀번호 : </label>
		            <input type = "password" name = "pass" value = "${dto.pass}" readonly>
		        </div>
		        <div class = "in">
		            <label>이름 : </label>
		            <input type = "text" name = "name" value = "${dto.name}" readonly>
		        </div>
		        <div class = "in">
		            <label>생성시점 : </label>
		            <input type = "datetime" name = "date" value = "${dto.date}"readonly >
		        </div>
		        <button type = "button" onclick="location.href = 'update.me'">나의정보 수정</button>
		        <button type = "button" onclick="location.href = 'delete.me'">나의정보 삭제</button>
		        <button type = "button" onclick="location.href = 'main.me'">메인페이지 이동</button>
		    </div>
        </c:when>    
	    <c:otherwise>
	        <c:redirect url="main.me"></c:redirect>
	    </c:otherwise>
    </c:choose>

</body>
</html>
```

- Update(나의정보 수정)
```html title:Update
<%@page import="com.itwillbs.domain.MemberDTO"%>
<%@page import="java.sql.Timestamp"%>

<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/update.jsp</title>
<style>
    .background {
        text-align: center;
        color: gray;
        margin: 30px auto;
        padding-left: 20px;
        width: 300px;
        height: auto;
        border: 1px solid #dadada;
        border-radius: 20px;
    }
    .in {
        display: flex;
        align-items: center;
        margin-top: 10px;
        margin-bottom: 10px;
    }
    .in label{
        width:88px;
        display: inline-block;
        text-align: left;
    }
    .background lebel{
        margin-right: 20px;
    }
    .background input {
        text-align: center;
        padding-left: 10px;
        color: gray;
        height: 40px;
        border: 1px solid #dadada;
        border-radius: 10px;
    }
    .background p{
        text-align: center;
        font-size: 20px;
        font-weight: bold;
    }
    button{
        margin-top: 5px;
        margin-bottom: 10px;
        color: gray;
        width: 200px;
        height: 30px;
        border: 1px solid #dadada;
        border-radius: 10px;
    }
</style>
</head>
<body>
   
    <c:if test="${!empty dto}">
    <div class = "background">
        <p>회원수정</p>
        <form action = "updatePro.me" method = "post">
        <div class = "in">
            <label>아이디 : </label>
            <input type = "text" name = "id" value = "${dto.id}" readonly>
        </div>
        <div class = "in">
            <label>비밀번호 : </label>
            <input type = "password" name = "pass" value = "${dto.pass}" readonly>
        </div>
        <div class = "in">
            <label>이름 : </label>
            <input type = "text" name = "name" value = "${dto.name}" >
        </div>
        <div class = "in">
            <label>생성시점 : </label>
            <input type = "datetime" name = "date" value = "${dto.date}>"readonly >
        </div>
        <button type = "submit" >저장</button>
        <button type = "button" onclick="location.href = 'main.me'">메인페이지 이동</button>
        </form>
    </div>
    </c:if>
    <c:if test="${empty request.dto}">
        <c:redirect url="main.me"></c:redirect>
    </c:if>

</body>
</html>
```

- Delete(회원 탈퇴)
```html title:Delete
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/Delete.jsp</title>
<style>
    .background {
        text-align: center;
        color: gray;
        margin: 30px auto;
        padding: 20px;
        width: 300px;
        height: auto;
        border: 1px solid #dadada;
        border-radius: 20px;
    }
    .in {
        display: flex;
        align-items: center;
        margin-top: 10px;
        margin-bottom: 10px;
    }
    .in label{
        width:88px;
        display: inline-block;
        text-align: left;
    }
    .background lebel{
        margin-right: 20px;
    }
    .background input {
        text-align: center;
        padding-left: 10px;
        color: gray;
        height: 40px;
        border: 1px solid #dadada;
        border-radius: 10px;
    }
    .background p{
        text-align: center;
        font-size: 20px;
        font-weight: bold;
    }
    .button input, button{
        margin-top: 5px;
        margin-bottom: 5px;
        color: gray;
        width: 200px;
        height: 30px;
        border: 1px solid #dadada;
        border-radius: 10px;
    }
</style>

</head>
<body>

    <c:if test="${! empty sessionScope.id }">
	    <form action = "deletePro.me" method = "post">
	        <div class = "background">
	            <p>회원탈퇴</p>
	            <div class = "in">
	                <label>아이디 : </label>
	                <input type = "text" name = "id" value = "${sessionScope.id}" readonly>
	            </div>
	            <div class = "in">
	                <label>비밀번호 : </label>
	                <input type = "password" name = "pass">
	            </div>
	
	            <div class = "button">
	               <input type = "submit" value = "삭제하기">
	            </div>
	            <div class = "button">
	               <button type = "button" onclick="location.href = 'info.me'">돌아가기</button>
	            </div>
	        </div>
	    </form>
    </c:if>
    <c:if test="${empty sessionScope.id }">
        <c:redirect url="info.me"></c:redirect>
    </c:if>
</body>
</html>
```

- Lsit(관리자일때 회원정보 조회)
```html title:List
<%@page import="com.itwillbs.domain.MemberDTO"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/List.jsp</title>
<style>
	.left-Sidebar {
	    padding-left: 8px;
	    margin-top: 20px;
	    width: 300px;
	    font-size: 18px;
	    text-align: center;
	    margin-top: 10px;
	    border: 1px solid #dadada;
	    border-radius: 20px;
	}
	.left-Sidebar p {
	   text-align:left;
	}
	button{
	    align-items: center;
	    margin-left: 100px;
	    margin-top: 20px;
        width:120px;
        font-size: 16px;
        border: 1px solid #dadada;
        border-radius: 6px;
        margin-bottom: 10px;
    }
    button:hover{
        background-color: #dadada;
    }
    p {
        margin: 10px;
        font-size: 16px;
        color: gray;
    }
</style>
</head>
<body>
    <c:forEach items="${arrayList}" var = "dto">
	 	<div class = "left-Sidebar">
	 	    <p>아이디 : ${dto.id}</p>
	 	    <p>비밀번호 : ${dto.pass}</p>
	        <p>이름 : ${dto.name}</p>
	        <p>생성날짜 : ${dto.date}</p>
	    </div>
	    </c:forEach>
    <button type = "button" onclick="location.href = 'main.me'">돌아가기</button>
</body>
</html>
```

##### board
- List(게시판 목록)
```HTML title:List

<%@page import="com.itwillbs.domain.MemberDTO"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>member/List.jsp</title>
<style>
	.left-Sidebar {
	    padding-left: 8px;
	    margin-top: 20px;
	    width: 300px;
	    font-size: 18px;
	    text-align: center;
	    margin-top: 10px;
	    border: 1px solid #dadada;
	    border-radius: 20px;
	}
	.left-Sidebar p {
	   text-align:left;
	}
	button{
	    align-items: center;
	    margin-left: 100px;
	    margin-top: 20px;
        width:120px;
        font-size: 16px;
        border: 1px solid #dadada;
        border-radius: 6px;
        margin-bottom: 10px;
    }
    button:hover{
        background-color: #dadada;
    }
    p {
        margin: 10px;
        font-size: 16px;
        color: gray;
    }
    .pageList{
        width: 460px;
        display: flex;
        justify-content: center;
        padding: 10px;
        align-items: center;
        
    }
    .pageNumber{
        vertical-align: middle;
    }
</style>
</head>
<body>
    <c:forEach items="${arrayList}" var = "dto">
	 	<div class = "left-Sidebar">
	 	    <p>아이디 : ${dto.id}</p>
	 	    <p>비밀번호 : ${dto.pass}</p>
	        <p>이름 : ${dto.name}</p>
	        <p>생성날짜 : ${dto.date}</p>
	    </div>
    </c:forEach>
    
    <div class = "pageList">
            <c:if test="${pageDTO.startPage > pageDTO.pageBlock}">
                <div class = "PageNumber">
                   <a href = "memberList.me?pageNum=${pageDTO.startPage - 10}">&lt&lt</a>
                </div>
            </c:if>
            
            <c:forEach begin="${pageDTO.startPage}" end="${pageDTO.endPage}" var="page">
                <div class = "PageNumber">
                   <a href = "memberList.me?pageNum=${page}">${page}</a>
               </div>
            </c:forEach>
            
            <c:if test="${pageDTO.endPage < pageDTO.pageCount}">
                <div class = "PageNumber">
                   <a href = "memberList.me?pageNum=${pageDTO.endPage + 10}">>></a>
                </div>
            </c:if>
        </div>
        
    <button type = "button" onclick="location.href = 'main.me'">돌아가기</button>
</body>
</html>

```

- read(게시글 읽기)
```html title:Read
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>board/Read.jsp</title>
<style>
    .background {
        margin:20px auto;
        width: 600px;
        padding:20px;
        text-align: left;
        border: 1px solid #dadada;
        border-radius: 20px;
    }
    h3 {
        text-align: center;
    }
    .content label{
        text-align: center;
    }
    div {
        margin: 10px;
    }
    
    input {
        padding-left: 10px;
        height: 26px;
        width: 80px;
        border: 1px solid #dadada;
        border-radius: 8px;
    
    }
    .button{
        display: flex;
        justify-content: flex-end;
    
    }
    button {
        margin: 10px;
        margin-bottom: 0px;
        height: 28px;
        width: 100px;
        border: 1px solid #dadada;
        border-radius: 8px;
    
    }
    button:hover{
        background-color: #dadada;
    }
    .subject{
        width: 500px;
    
    }
    textarea {
        height:400px;
        width: 500px;
        padding-left: 10px;
        padding-top: 10px;
        border: 1px solid #dadada;
        border-radius: 8px;
        resize: none;
        vertical-align: top;
    }
</style>
</head>
<body>
        <c:if test="${!empty requestScope.dto }">
        <form action = "Delete.jsp" method = "post">
        <div class = "background">
            <h3>글 읽 기</h3>
            <div>
                <label>글쓴이 : </label>
                <input type = "text" name = "name" value = "${dto.name}" readonly>
            </div>
            <div>
                <label>글제목 : </label>
                <input type = "text" name = "subject" value = "${dto.subject}" class = "subject" readonly>
            </div>
            <div>
                <label class = "content">글내용 : </label>
                <textarea name = "content" readonly>${dto.content}</textarea>
            </div>
            <c:if test="${dto.name eq sessionScope.id }">
	            <div class = "button">
	                <button type = "button" onclick = "location.href = 'read_update.bo?num=${dto.num}'">수정하기</button>
	            </div>
	            <div class = "button">
	                <button type = "button" onclick = "location.href = 'read_delete.bo?num=${dto.num}'">삭제하기</button>
	            </div>
            </c:if>
            <div class = "button">
                <button type = "button" onclick = "location.href = 'list.bo'">돌아가기</button>
            </div>
        </div>
    </form>
    </c:if>
</body>
</html>
```

- readUpdate(게시글 수정 - 게시글 작성자만 가능)
```html title:ReadUpdate
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta http-equiv="Refresh" content="86400">
<title>board/Read_Update.jsp</title>
<style>
    .background {
        margin:20px auto;
        width: 600px;
        padding:20px;
        text-align: left;
        border: 1px solid #dadada;
        border-radius: 20px;
    }
    h3 {
        text-align: center;
    }
    .content label{
        text-align: center;
    }
    div {
        margin: 10px;
    }
    
    input {
        padding-left: 10px;
        height: 26px;
        width: 80px;
        border: 1px solid #dadada;
        border-radius: 8px;
    
    }
    .button{
        display: flex;
        justify-content: flex-end;
    
    }
    button, .button input{
        margin: 10px;
        margin-bottom: 0px;
        height: 28px;
        width: 100px;
        border: 1px solid #dadada;
        border-radius: 8px;
    
    }
    button:hover{
        background-color: #dadada;
    }
    .subject{
        width: 500px;
    
    }
    textarea {
        height:400px;
        width: 500px;
        padding-left: 10px;
        padding-top: 10px;
        border: 1px solid #dadada;
        border-radius: 8px;
        resize: none;
        vertical-align: top;
    }
</style>
</head>
<body>
        <c:if test="${!empty requestScope.dto }">
        <form action = "read_updatePro.bo" method = "post">
        <div class = "background">
            <h3>글 수 정</h3>
            <div>
                <label>글쓴이 : </label>
                <input type = "text" name = "name" value = "${dto.name}" readonly>
            </div>
            <div>
                <label>글제목 : </label>
                <input type = "text" name = "subject" value = "${dto.subject}" class = "subject">
            </div>
            <div>
                <label class = "content">글내용 : </label>
                <textarea name = "content" id = "id_edit">${dto.content }</textarea>
            </div>
            <c:if test="${sessionScope.id eq dto.name }">
                <div class = "button">
                    <input type = "submit" value = "저장하기">
                </div>
                <input type = "hidden" name = "num" value="${dto.num }">
            </c:if>
            
            <div class = "button">
                <button type = "button" onclick = "location.href = 'list.bo'">돌아가기</button>
            </div>
        </div>
    </form>
    </c:if>
</body>
</html>
```

- write(글쓰기)
```html title:Write
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta http-equiv="Refresh" content="86400">
<title>board/Write.jsp</title>
<style>
	.background {
		margin:20px auto;
	    width: 600px;
	    padding:20px;
	    text-align: left;
	    border: 1px solid #dadada;
	    border-radius: 20px;
	}
	h3 {
		text-align: center;
	}
	.content label{
		text-align: center;
	}
	div {
		margin: 10px;
	}
	
	input, button {
		padding-left: 10px;
		height: 26px;
		width: 80px;
		border: 1px solid #dadada;
		border-radius: 8px;
	
	}
	.button, button{
		margin: 10px 10px 10px 238px;
		height: 28px;
		width: 100px;
	
	}
	.button input:hover, button:hover{
		background-color: #dadada;
	}
	.subject{
		width: 500px;
	
	}
	textarea {
		height:400px;
		width: 500px;
		padding-left: 10px;
		padding-top: 10px;
		border: 1px solid #dadada;
		border-radius: 8px;
		resize: none;
		vertical-align: top;
	}

</style>
</head>
<body>

	<c:if test="${! empty sessionScope.id }">
	<form action = "writePro.bo" method = "post">
		<div class = "background">
			<h3>글 쓰 기</h3>
			<div>
				<label>글쓴이 : </label>
				<input type = "text" name = "name" value = "${sessionScope.id }" readonly>
			</div>
			<div>
				<label>글제목 : </label>
				<input type = "text" name = "subject" class = "subject">
			</div>
			<div>
				<label class = "content">글내용 : </label>
				<textarea name = "content"></textarea>
			</div>
			<div class = "button">
				<input type = "submit" value = "저장" class = "button">
				
			</div>		
			<div class = "button">
			     <button type = "button" onclick = "location.href = 'main.me'">돌아가기</button>
			</div>
		</div>
	</form>
	</c:if>

</body>
</html><%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta http-equiv="Refresh" content="86400">
<title>board/Write.jsp</title>
<style>
	.background {
		margin:20px auto;
	    width: 600px;
	    padding:20px;
	    text-align: left;
	    border: 1px solid #dadada;
	    border-radius: 20px;
	}
	h3 {
		text-align: center;
	}
	.content label{
		text-align: center;
	}
	div {
		margin: 10px;
	}
	
	input, button {
		padding-left: 10px;
		height: 26px;
		width: 80px;
		border: 1px solid #dadada;
		border-radius: 8px;
	
	}
	.button, button{
		margin: 10px 10px 10px 238px;
		height: 28px;
		width: 100px;
	
	}
	.button input:hover, button:hover{
		background-color: #dadada;
	}
	.subject{
		width: 500px;
	
	}
	textarea {
		height:400px;
		width: 500px;
		padding-left: 10px;
		padding-top: 10px;
		border: 1px solid #dadada;
		border-radius: 8px;
		resize: none;
		vertical-align: top;
	}
</style>
</head>
<body>

	<c:if test="${! empty sessionScope.id }">
	<form action = "writePro.bo" method = "post">
		<div class = "background">
			<h3>글 쓰 기</h3>
			<div>
				<label>글쓴이 : </label>
				<input type = "text" name = "name" value = "${sessionScope.id }" readonly>
			</div>
			<div>
				<label>글제목 : </label>
				<input type = "text" name = "subject" class = "subject">
			</div>
			<div>
				<label class = "content">글내용 : </label>
				<textarea name = "content"></textarea>
			</div>
			<div class = "button">
				<input type = "submit" value = "저장" class = "button">
				
			</div>		
			<div class = "button">
			     <button type = "button" onclick = "location.href = 'main.me'">돌아가기</button>
			</div>
		</div>
	</form>
	</c:if>

</body>
</html>
```

---
#### JAVA 파일
##### member
- MemberController (가상 주소 맵핑 작업)
```Java title:MemberController
package com.itwillbs.controller;

import java.io.IOException;

import org.apache.catalina.connector.Response;

import com.itwillbs.domain.PageDTO;
import com.itwillbs.service.MemberService;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class MemberController extends HttpServlet{
	
	MemberService memberService = new MemberService();
	
	//HttpServlet 자바 서블릿 파일 => 모든 파일 내용을 가져와서 사용 => 상속
	//servlet 을 가져와서 => servlet 파일이 됨 => 상속 받아옴
	//HttpServlet 서블릿(처리담당자) 동작원리 => service(), doGet(), doPost() 메서드 자동으로 호출

	//doGet(), doPost() 재정의 => 메서드 오버라이딩
	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse rseponse) throws ServletException, IOException {
		//get 방식으로 요청이 들어오면 자동으로 실행되는 메서드 doGet()
		System.out.println("MemberController doGet()");
		doProcess(request, rseponse);
	}
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse rseponse) throws ServletException, IOException {
		//post 방식으로 요청이 들어오면 자동으로 실행되는 메서드 doPost()
		System.out.println("MemberController doPost()");
		doProcess(request, rseponse);
	}
	
	//get방식, post방식 상관없이 하나의 메서드로 구현 doProcess()로 구현 
	//주소 맵핑처리
	protected void doProcess(HttpServletRequest request, HttpServletResponse rseponse) throws ServletException, IOException {
		//post 방식으로 요청이 들어오면 자동으로 실행되는 메서드 doPost()
		System.out.println("MemberController doProcess()");
		
		//1) 가상주소 뽑아오기
		String sPath = request.getServletPath();
		System.out.println("가상주소 불러오기 : " + sPath);
		
		RequestDispatcher dispatcher = null;
		//2) 뽑아온 주소와 예상하는 가상주소 비교
		switch (sPath) {
			//가상주소 -> 실제 주소 연결 (insert.jsp)
			//이동방식
			//	1)하이퍼링크 -> 주소가 변경되면서 이동
			// 		rseponse.sendRedirect("0.member/Insert.jsp");
			//	2)주소가 변경없이 이동 -> 자바파일 이용해서 이동
			//		자바파일 객체생성
			// 		메서드 호출
			case "/insert.me" : System.out.println("/insert.me 가상주소 비교 일치");
				dispatcher = request.getRequestDispatcher("0.member/Insert.jsp");
				dispatcher.forward(request, rseponse);
			break;
				
			case "/insertPro.me" : System.out.println("/insertPro.me 가상주소 비교 일치"); 
				//회원가입 처리 작업 메서드 처리 호출()
				memberService.insertMember(request);
				//로그인 페이지 이동
				//하이퍼링크 이동
				rseponse.sendRedirect("login.me"); //login.me 로 url 가림
			break;
				
			case "/login.me" : System.out.println("/login.me 가상주소 비교 일치");
				dispatcher = request.getRequestDispatcher("0.member/Login.jsp"); 
				dispatcher.forward(request, rseponse);
			break;
				
			case "/loginPro.me" : System.out.println("/loginPro.me 가상주소 비교 일치");
				if(memberService.loginCheck(request)) {
					System.out.println("아이디 비밀번호 체크 성공");
					HttpSession session = request.getSession();
					session.setAttribute("id", request.getParameter("id"));
					rseponse.sendRedirect("main.me");
				}
				else {
					System.out.println("아이디 비밀번호 체크 실패");
					rseponse.sendRedirect("login.me");
				}
			break;
				
			case "/main.me" : System.out.println("/main.me 가상주소 비교 일치");
				dispatcher = request.getRequestDispatcher("0.member/Main.jsp"); 
				dispatcher.forward(request, rseponse);
			break;
				
			case "/logout.me" : System.out.println("/logout.me 가상주소 비교 일치");
				memberService.logOut(request);
				rseponse.sendRedirect("login.me");
			break;
				
			case "/info.me" : System.out.println("/info.me 가상주소 비교 일치");
				memberService.getmember(request);
				dispatcher = request.getRequestDispatcher("0.member/Info.jsp");
				dispatcher.forward(request, rseponse);
			break;
				
			case "/delete.me" : System.out.println("/delete.me 가상주소 비교 일치");
				memberService.getmember(request);
				dispatcher = request.getRequestDispatcher("0.member/Delete.jsp");
				dispatcher.forward(request, rseponse);
			break;
				
			case "/deletePro.me" : System.out.println("/detetePro.me 가상주소 비교 일치");
				if(memberService.loginCheck(request)) {
					memberService.delete(request);
					System.out.println("회원삭제 완료");
					HttpSession session = request.getSession();
					session.invalidate();
					rseponse.sendRedirect("main.me");
				}
				else {
					System.out.println("비밀번호 오류");
					rseponse.sendRedirect("delete.me");
				}
			break;
				
			case "/update.me" : System.out.println("/update.me 가상주소 비교 일치");
				HttpSession session = request.getSession();
				if(session.getAttribute("id") != null) {
					memberService.getmember(request);
					dispatcher = request.getRequestDispatcher("0.member/update.jsp");
					dispatcher.forward(request, rseponse);
				}
				else {
					rseponse.sendRedirect("main.me");
				}
			break;
				
			case "/updatePro.me" : System.out.println("/updatePro.me 가상주소 비교 일치");
				memberService.update(request);
				System.out.println("수정완료");
				rseponse.sendRedirect("info.me");
			break;
				
			case "/memberList.me" : System.out.println("/memberList.me 가상주소 비교 일치");
				HttpSession session1 = request.getSession();
				if(session1.getAttribute("id").equals("root")) {
					
					//한 페이지에 보이는 글 개수 설정 -> 10 설정
					int pageSize = 10;
					
					//페이지 번호 가져오기
					String pageNum = request.getParameter("pageNum");
					
					//페이지 번호 정수형으로 변경
					int curentPage = Integer.parseInt(pageNum);
					
					//페이징 관련 데이터 저장하는 PageDTO
					PageDTO pageDTO = new PageDTO();
					pageDTO.setPageNum(pageNum);
					pageDTO.setCurentPage(curentPage);
					pageDTO.setPageSize(pageSize);
					
					//getBoardCount() 메서드 호출
					pageDTO.setCount(memberService.getMemberCount());
					
					pageDTO.setCount(200);
					//page 이동관련 로직
					int pageBlock = 10;
					int startPage = ((curentPage - 1) / pageBlock) * pageBlock + 1;
					int endPage = startPage + pageBlock - 1;
					int pageCount =  pageDTO.getCount() / pageBlock + (pageDTO.getCount() % pageBlock == 0 ? 0 : 1);
					
					if(endPage > pageCount) {
						endPage = (int) pageCount;
					}
					
					pageDTO.setStartPage(startPage);
					pageDTO.setEndPage(endPage);
					pageDTO.setPageCount(pageCount);
					pageDTO.setPageBlock(pageBlock);
					
					request.setAttribute("pageDTO", pageDTO);
					
					memberService.memberList(request, pageDTO);
									
					System.out.println("회원목록 조회 완료");
					dispatcher = request.getRequestDispatcher("0.member/List.jsp");
					dispatcher.forward(request, rseponse);
				}
				else {
					rseponse.sendRedirect("main.me");
				}
			break;
								
				
			default:
				break;
		}
	}	
}

```

- MemberService(서비스 역활 - Controller역할)
```Java title:MemberService
package com.itwillbs.service;

import java.sql.Timestamp;
import java.util.ArrayList;

import com.itwillbs.dao.BoardDAO;
import com.itwillbs.dao.MemberDAO;
import com.itwillbs.domain.MemberDTO;
import com.itwillbs.domain.PageDTO;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpSession;

public class MemberService {
	MemberDAO dao = new MemberDAO();
	
	//회원가입(insertPro.jsp에 해당되는 내용)
	public void insertMember(HttpServletRequest request) {
		System.out.println("Service insertMember() <<");
		
	    String id = request.getParameter("id");
	    String pass = request.getParameter("pass");
	    String name = request.getParameter("name");
	    Timestamp date = new Timestamp(System.currentTimeMillis());

	    MemberDTO dto = new MemberDTO();
	    dto.setId(id);
	    dto.setPass(pass);
	    dto.setName(name);
	    dto.setDate(date);
		
	    dao = new MemberDAO();
	    dao.insertMember(dto);
	}
	//로그인체크(LoginPro.jsp에 해당되는 내용)
	public boolean loginCheck(HttpServletRequest request) {
		System.out.println("Service loginCheck() <<");
		boolean isCheck = false;
		String id = request.getParameter("id");
	    String pass = request.getParameter("pass");
	    
	    dao = new MemberDAO();
	    
	    if(dao.userCheck(id, pass)){
	    	return isCheck = true;
	    }
	    else{
	        return isCheck;
	    }
	}
	//로그아웃(Logout.jsp에 해당되는 내용)
	public void logOut(HttpServletRequest request) {
		HttpSession session = request.getSession();
		session.invalidate();
	}
	//회원정보(info.jsp에 해당되는 내용)
	public void getmember(HttpServletRequest request) {
		HttpSession session = request.getSession();
		dao = new MemberDAO();
		MemberDTO dto = dao.getMember((String)session.getAttribute("id"));
		request.setAttribute("dto", dto);
	}
	//회원삭제(delete.jsp에 해당되는 내용)
	public void delete(HttpServletRequest request) {
		String id = request.getParameter("id");
		dao = new MemberDAO();
		dao.deleteMember(id);
	}
	//회원정보수정(updatePro.jsp에 해당되는 내용)
	public void update(HttpServletRequest request) {
	    dao = new MemberDAO();
	    MemberDTO dto = new MemberDTO();
	    dto.setId(request.getParameter("id"));
	    dto.setPass(request.getParameter("pass"));
	    dto.setName(request.getParameter("name"));
	    dto.setDate(new Timestamp(System.currentTimeMillis()));
	    dao.updateMember(dto);
	}
	//회원리스트(list.jsp에 해당되는 내용)
	public void memberList(HttpServletRequest request, PageDTO pageDTO) {	
		
		System.out.println("MemberService getMemberList() 메서드 호출");
		//시작하는 행번호 구하기 (한 화면에 보여줄 글 개수 pageSize, 현 페이지 번호 curentPage)
		int pageSize = pageDTO.getPageSize();
		int curentPage = pageDTO.getCurentPage();
		int startRow = ((curentPage - 1) * pageSize) + 1;
		pageDTO.setStartRow(startRow);
		//끝나는 행번호 구하기
		int endRow = (startRow + pageSize) - 1;
		pageDTO.setEndRow(endRow);
		
	    dao = new MemberDAO();
		ArrayList<MemberDTO> resultDTO = dao.getMemberList(pageDTO);
		request.setAttribute("arrayList", resultDTO);
	}
	
	//글 전체 개수 가져오기
	public int getMemberCount() {
		MemberDAO dao = new MemberDAO();
		return dao.getMemberCount();
	}
}
```

- MemberDAO(SQL 디비조회 및 결과값 -> Model)
```java title:MemberDAO
package com.itwillbs.dao;


import java.sql.Connection;

import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.ArrayList;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

import com.itwillbs.domain.MemberDTO;
import com.itwillbs.domain.PageDTO;



public class MemberDAO {
	// 멤버변수 
	
	Connection con = null;
	PreparedStatement insert = null;
	PreparedStatement select = null;
	PreparedStatement delete = null;
	PreparedStatement update = null;
	ResultSet result = null;
	
	
	// 메서드 (멤버함수) : 기능을 구현(명령 모임)
	//Close 호출 메서드
	public void dbClose() {

		if(result != null) {
			try { result.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(select != null) {
			try { select.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(insert != null) {
			try { insert.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(delete != null) {
			try { delete.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(update != null) {
			try { update.close(); }
			catch (SQLException e1) { e1.printStackTrace(); }
		}
		if(con != null) {
			try { con.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
	}
	
	// 회원가입하는 기능 정의
	//throws SQLException, ClassNotFoundException
	public Connection getConnection() throws Exception {

		//import javax.naming.Context;
		//import javax.naming.InitialContext;
		Context init = new InitialContext();
		
		//context.xml 자원의장소/자원의이름
		DataSource ds = (DataSource)init.lookup("java:comp/env/jdbc/MysqlDB");
		con = ds.getConnection();
		
		return con;
	}
	//회원가입
	public void insertMember(MemberDTO dto)  {

		System.out.println("MemberDAO insertMember() <<");
		System.out.println(dto.getId());
		System.out.println(dto.getPass());
		System.out.println(dto.getName());
		System.out.println(dto.getDate());
		
		String id = dto.getId();
		String pass = dto.getPass();
		String name = dto.getName();
		Timestamp date = dto.getDate();
		
		try {
			con = getConnection();
			String sql_Insert = "INSERT INTO members(id, pass, name, date) VALUES (?, ?, ?, ?)";
			insert = con.prepareStatement(sql_Insert);
			insert.setString(1, id);
			insert.setString(2, pass);
			insert.setString(3, name);
			insert.setTimestamp(4, date);
			insert.executeUpdate();
		}
		catch (Exception e) {
			//Exception 의 내용을 프린트 스텍 트레이스로 출력한다!
			e.printStackTrace();
		} 
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			dbClose();
		}
	}
	//로그인 체크
	public boolean userCheck(String id, String pass) {
		
		System.out.println("MemberDAO userCheck() <<");
		try {
			con = getConnection();
			String sql_Select = "SELECT * FROM members WHERE id = ? AND pass = ?";
			select = con.prepareStatement(sql_Select);
			select.setString(1, id);
			select.setString(2, pass);
			result =  select.executeQuery();
			if (result.next()) {
				return true;
			}		
		}
		catch (Exception e) { e.printStackTrace(); }
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			dbClose();
		}
		return false;
	}
	//회원정보 확인
	public MemberDTO getMember (String id) {

		MemberDTO dto = new MemberDTO();
		try {
			con = getConnection();
			String sql_Select_All = "SELECT * FROM members WHERE id = ?";
		    select = con.prepareStatement(sql_Select_All);
		    select.setString(1, id);
		    result = select.executeQuery();
		    if(result.next()) {
		    	dto.setId(result.getString("id"));
		    	dto.setPass(result.getString("pass"));
		    	dto.setName(result.getString("name"));
		    	dto.setDate(result.getTimestamp("date"));
		    	return dto;
		    }
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			dbClose();
		}
		
		return dto = null;
	}
	//회원삭제
	public void deleteMember(String id) {
		try {
			con = getConnection();
			String sql_delete = "DELETE FROM members WHERE id = ?";
			delete = con.prepareStatement(sql_delete);
			delete.setString(1, id);
			delete.executeUpdate();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			dbClose();
		}
	}
	//회원정보 수정
	public void updateMember(MemberDTO dto) {
		try {
			con = getConnection();
			String sql_Update = "UPDATE members SET name = ?, date = ? WHERE id = ? ";
			update = con.prepareStatement(sql_Update);
			update.setString(1, dto.getName());
			update.setTimestamp(2, dto.getDate());
			update.setString(3, dto.getId());
			update.executeUpdate();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			dbClose();
		}
	}
	//전체회원 리스트(어레이 리스트) -> 회원이 몇명인지 모르기 때문에 리스트가 확실한거 같음
	//전체회원목록 가져오기(ArrayList 객체 반환)
	public ArrayList<MemberDTO> getMemberList(PageDTO pageDTO){

		ArrayList<MemberDTO> resultDTO = new ArrayList<MemberDTO>();
		try {
			con = getConnection();
//			String sql_Select = "SELECT * FROM members";
			String sql_Select = "SELECT * FROM members ORDER BY date desc LIMIT ?, ?";
			select = con.prepareStatement(sql_Select);
			select.setInt(1, pageDTO.getStartRow());
			select.setInt(2, pageDTO.getEndRow());
			result = select.executeQuery();
			while (result.next()) {
				MemberDTO dto = new MemberDTO();
				dto.setId(result.getString("id"));
				dto.setPass(result.getString("pass"));
				dto.setName(result.getString("name"));
				dto.setDate(result.getTimestamp("date"));
				resultDTO.add(dto);
			}
			
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			//예외발생 상관없이 마무리 작업 영역
			//Connection con , PreparedStatement insert -> 해제 작업 
			dbClose();
		}
		
		return resultDTO;
	}
	
	//글 전체 개수 가져오기
	public int getMemberCount() {
		int count = 0;
		String sql_getBoardCount = "select count(*) as count from members";
		try {
			con = getConnection();
			select = con.prepareStatement(sql_getBoardCount);
			result = select.executeQuery();
			if(result.next()) {
				count = result.getInt("count");
			}
			
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
		return count;
	}
}
```

- MemberDTO(DAO에서 가져온객체를 받는 곳 -> Model)
```java title:MemberDTO
package com.itwillbs.domain;

import java.sql.Timestamp;

public class MemberDTO {
	
	//멤버변수
	//중요한 데이터를 담아서 전달하기 위해서 외부 접근 못하게 설정
	private String id;
	private String pass;
	private String name;
	private Timestamp date;
	
	
	//생성자(생략 초기값 할당)
	
	//메서드(멤버 함수)
	
	
	//getter AND setter (alt + shift + s -> r)
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	
	public String getPass() {
		return pass;
	}
	public void setPass(String pass) {
		this.pass = pass;
	}
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	
	public Timestamp getDate() {
		return date;
	}
	public void setDate(Timestamp date) {
		this.date = date;
	}
}

```

##### board
- BoardController(가상주소 맵핑 작업 -> Controller)
```java title:BoardController
package com.itwillbs.controller;

import java.io.IOException;

import com.itwillbs.domain.PageDTO;
import com.itwillbs.service.BoardService;
import com.mysql.cj.x.protobuf.MysqlxCrud.Delete;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class BoardController extends HttpServlet{
	
	BoardService service = new BoardService();

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse rseponse) throws ServletException, IOException {
		//get 방식으로 요청이 들어오면 자동으로 실행되는 메서드 doGet()
		System.out.println("BoardController doGet()");
		doProcess(request, rseponse);
	}
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse rseponse) throws ServletException, IOException {
		//post 방식으로 요청이 들어오면 자동으로 실행되는 메서드 doPost()
		System.out.println("BoardController doPost()");
		doProcess(request, rseponse);
	}
	
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
		String path = request.getServletPath();
		RequestDispatcher dispatcher = null;
		
		switch(path) {
			case "/write.bo":
				System.out.println("/write.bo <<가상주소 일치");
				dispatcher = request.getRequestDispatcher("0.board/Write.jsp");
				dispatcher.forward(request, response);
			break;
			
			case "/writePro.bo" :
				System.out.println("/write.bo <<가상주소 일치");
				HttpSession session = request.getSession();
				if(session.getAttribute("id") != null) {
					service.writeInsert(request, response);
					response.sendRedirect("main.me");
				}
				else {
					response.sendRedirect("login.me");
				}
			break;
			
			case "/list.bo" :
				System.out.println("/list.bo <<가상주소 일치");
				//한 페이지에 보이는 글 개수 설정 -> 10 설정
				int pageSize = 10;
				
				//페이지 번호 가져오기
				String pageNum = request.getParameter("pageNum");
				
				//페이지 번호 정수형으로 변경
				int curentPage = Integer.parseInt(pageNum);
				
				//페이징 관련 데이터 저장하는 PageDTO
				PageDTO pageDTO = new PageDTO();
				pageDTO.setPageNum(pageNum);
				pageDTO.setCurentPage(curentPage);
				pageDTO.setPageSize(pageSize);
				
				//getBoardCount() 메서드 호출
				pageDTO.setCount(service.getBoardCount());
				
				//page 이동관련 로직
				int pageBlock = 10;
				int startPage = ((curentPage - 1) / pageBlock) * pageBlock + 1;
				int endPage = startPage + pageBlock - 1;
				int pageCount =  pageDTO.getCount() / pageBlock + (pageDTO.getCount() % pageBlock == 0 ? 0 : 1);
				
				if(endPage > pageCount) {
					endPage = (int) pageCount;
				}
				
				pageDTO.setStartPage(startPage);
				pageDTO.setEndPage(endPage);
				pageDTO.setPageCount(pageCount);
				pageDTO.setPageBlock(pageBlock);
				
				request.setAttribute("pageDTO", pageDTO);

				
				service.getBoardList(request, pageDTO);
				dispatcher = request.getRequestDispatcher("0.board/List.jsp");
				dispatcher.forward(request, response);
			break;
			
			case "/read.bo" :
				System.out.println("/read.bo <<가상주소 일치");
				service.getBoard(request);
				service.readCount(request);
				dispatcher = request.getRequestDispatcher("0.board/Read.jsp");
				dispatcher.forward(request, response);
			break;
			
			case "/read_update.bo" :
				System.out.println("/read_update.bo <<가상주소 일치");
				if(service.userCheck(request)) {
					service.getBoardUpdate(request);
					dispatcher = request.getRequestDispatcher("0.board/Read_Update.jsp");
					dispatcher.forward(request, response);
				}
				else {
					response.sendRedirect("list.bo");
				}		
			break;
			
			case "/read_updatePro.bo" :
				if(service.userCheck(request)) {
					System.out.println("/read_updatePro.bo <<가상주소 일치");
					service.setBoardUpdate(request);
					response.sendRedirect("list.bo");
				}
				else {
					response.sendRedirect("list.bo");
				}
			break;
			
			case "/read_delete.bo" :
				System.out.println("/read_delete.bo <<가상주소 일치");
				if(service.userCheck(request)) {
					service.deleteBoard(request);
					response.sendRedirect("list.bo");
				}
				else {
					response.sendRedirect("list.bo");
				}
			break;
		}
	}
}
```

- boardService( controller 작업 -> Controller)
```java title:BoardService
package com.itwillbs.service;

import java.sql.Timestamp;
import java.util.ArrayList;

import com.itwillbs.dao.BoardDAO;
import com.itwillbs.domain.BoardDTO;
import com.itwillbs.domain.PageDTO;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class BoardService {
	
	//글쓰기
	public void writeInsert(HttpServletRequest request, HttpServletResponse response) {

		System.out.println("BoardService writeInsert() 메서드 호출");
		BoardDAO dao = new BoardDAO();
	    BoardDTO dto = new BoardDTO();
	    
	    int num = dao.numCount();
	    num += 1;
	    Timestamp date = new Timestamp(System.currentTimeMillis());
	    dto.setNum(num);
	    dto.setName(request.getParameter("name")); 
	    dto.setSubject(request.getParameter("subject"));
	    dto.setContent(request.getParameter("content"));
	    dto.setReadCount(0);
	    dto.setDate(date);
	    dao.insertBoard(dto);
	
	    System.out.println("글쓰기 완료");
        
	}
	
	//글 전체 가져오기
	public void getBoardList(HttpServletRequest request, PageDTO pageDTO) {

		System.out.println("BoardService getBoardList() 메서드 호출");
		//시작하는 행번호 구하기 (한 화면에 보여줄 글 개수 pageSize, 현 페이지 번호 curentPage)
		int pageSize = pageDTO.getPageSize();
		int curentPage = pageDTO.getCurentPage();
		int startRow = ((curentPage - 1) * pageSize) + 1;
		pageDTO.setStartRow(startRow);
		//끝나는 행번호 구하기
		int endRow = (startRow + pageSize) - 1;
		pageDTO.setEndRow(endRow);
		
	    BoardDAO dao = new BoardDAO();
	    ArrayList<BoardDTO> resultDTO = dao.getBoardList(pageDTO);
		request.setAttribute("arrayList", resultDTO);
	}
	
	//글 읽기
	public void getBoard (HttpServletRequest request) {
		System.out.println("BoardService getBoard() 메서드 호출");
		int num = Integer.parseInt(request.getParameter("num"));
	    BoardDAO dao = new BoardDAO();
	    BoardDTO dto = dao.getBoard(num);
	    request.setAttribute("dto", dto);
	}	
	
	//글 수정페이지 불러오기
	public void getBoardUpdate(HttpServletRequest request) {
		System.out.println("BoardService getBoardUpdate() 메서드 호출");
		
	    int num = Integer.parseInt(request.getParameter("num"));
        BoardDAO dao = new BoardDAO();
        BoardDTO dto = dao.getBoard(num);
        request.setAttribute("dto", dto);
	}
	
	//글 수정하기
	public void setBoardUpdate(HttpServletRequest request) {
		System.out.println("BoardService setBoardUpdate() 메서드 호출");
	    int num = Integer.parseInt((request.getParameter("num")));
	    String subject = request.getParameter("subject");
	    String content = request.getParameter("content");
	    BoardDAO dao = new BoardDAO();
	    BoardDTO dto = new BoardDTO();
	    dto.setNum(num);
	    dto.setSubject(subject);
	    dto.setContent(content);
	    dao.update(dto);
	}
	
	//글 삭제하기
	public void deleteBoard(HttpServletRequest request) {
		System.out.println("BoardService deleteBoard() 메서드 호출");
		int num = Integer.parseInt(request.getParameter("num"));
	    BoardDAO dao = new BoardDAO();
	    dao.delete(num);
	}
	
	//조회수 카운트
	public void readCount(HttpServletRequest request) {
		System.out.println("BoardService readCount() 메서드 호출");
		BoardDAO dao = new BoardDAO();
		int num = Integer.parseInt(request.getParameter("num"));
		dao.readCount(num);
	}
	
	//유저 체크
	public boolean userCheck(HttpServletRequest request) {
		boolean isCheck = false;
		HttpSession session = request.getSession();	
		if(session.getAttribute("id") != null) {
			isCheck = true;
		}
		return isCheck;
	}
	
	//글 전체 개수 가져오기
	public int getBoardCount() {
		BoardDAO dao = new BoardDAO();
		return dao.getBoardCount();
	}
}
```

- BoardDAO ( 디비작업 - > Model)
```java title:BoardDAO
package com.itwillbs.dao;

import java.sql.Connection;


import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

import com.itwillbs.domain.BoardDTO;
import com.itwillbs.domain.PageDTO;

public class BoardDAO {
	
	Connection con = null;
	PreparedStatement insert = null;
	PreparedStatement select = null;
	PreparedStatement delete = null;
	PreparedStatement update = null;
	ResultSet result = null;
		
	public void dbClose() {

		if(result != null) {
			try { result.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(select != null) {
			try { select.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(insert != null) {
			try { insert.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(delete != null) {
			try { delete.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(update != null) {
			try { update.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		if(con != null) {
			try { con.close(); }
			catch (SQLException e) { e.printStackTrace(); }
		}
		
	}

	public Connection getConnection() throws Exception {

		//import javax.naming.Context;
		//import javax.naming.InitialContext;
		Context init = new InitialContext();
		//context.xml 자원의장소/자원의이름
		DataSource ds = (DataSource)init.lookup("java:comp/env/jdbc/MysqlDB");
		Connection con = ds.getConnection();
		return con;
	}
	//numCount 값 리턴해주기
	public int numCount() {

		int count = 0;
		try {
			con = getConnection();
			String sql_Select_num = "SELECT MAX(num) as num FROM board";
		    select = con.prepareStatement(sql_Select_num);
		    result =  select.executeQuery();
			while(result.next()) {
				count = result.getInt("num");
			}
		}catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
		return count;
	}
	//readCount 조회수 올려주기 
	public void readCount(int num) {
		try {
			con = getConnection();
			String sql_Update = "UPDATE board SET readcount = readcount + 1 WHERE num = ?";
			update = con.prepareStatement(sql_Update);
			update.setInt(1, num);
			update.executeUpdate();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
	}
	//Insert 해주기 디비에 글쓴값을 BoardDTO 에서 받아와서
	public void insertBoard(BoardDTO dto) {

		try {
			con = getConnection();
			String sql_Insert = "INSERT INTO board(num, name, subject, content, readcount, date) VALUES(?, ?, ?, ?, ?, ?)";
		    insert = con.prepareStatement(sql_Insert);
		    insert.setInt(1, dto.getNum());
		    insert.setString(2, dto.getName());
		    insert.setString(3, dto.getSubject());
		    insert.setString(4, dto.getContent());
		    insert.setInt(5, dto.getReadCount());
		    insert.setTimestamp(6, dto.getDate());
		    insert.executeUpdate();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}	
	}
	//게시판 에 전체 글을 보여주기(selectAll 하기)
	public ArrayList<BoardDTO> getBoardList(PageDTO pageDTO){

		ArrayList<BoardDTO> dto = new ArrayList<BoardDTO>();
		try {
			con = getConnection();
//			String sql_Select_All = "SELECT * FROM board ORDER BY num desc";
			String sql_Select_All = "SELECT * FROM board ORDER BY num desc LIMIT ?, ?";
		    select = con.prepareStatement(sql_Select_All);
		    select.setInt(1, pageDTO.getStartRow());
		    select.setInt(2, pageDTO.getEndRow());
		    result = select.executeQuery();
		    while(result.next()) {
		    	BoardDTO dtoBoard = new BoardDTO();
		    	dtoBoard.setNum(result.getInt("num"));
		    	dtoBoard.setName(result.getString("name"));
		    	dtoBoard.setSubject(result.getString("subject"));
		    	dtoBoard.setContent(result.getString("content"));
		    	dtoBoard.setReadCount(result.getInt("readcount"));
		    	dtoBoard.setDate(result.getTimestamp("date"));
		    	dto.add(dtoBoard);
	    	}
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
		return dto;
	}
	//글 읽기 num 을 기준으로 그 번호로 된 글 가져오기
	public BoardDTO getBoard (int num) {

		BoardDTO dto = null;
		try {
			con = getConnection();
			String sql_Select_Num = "SELECT * FROM board WHERE num = ?";
			select = con.prepareStatement(sql_Select_Num);
			select.setInt(1, num);
			result = select.executeQuery();
			
			if(result.next()){
				dto = new BoardDTO();
				dto.setNum(result.getInt("num"));
				dto.setName(result.getString("name"));
				dto.setSubject(result.getString("subject"));
				dto.setContent(result.getString("content"));
				dto.setReadCount(result.getInt("readcount"));
				dto.setDate(result.getTimestamp("date"));
			}
			
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
		
		return dto;
	}
	//글 삭제하기
	public void delete(int num) {
		try {
			 con = getConnection();
			String sql_Delete = "DELETE FROM board WHERE num = ?";
		    delete = con.prepareStatement(sql_Delete);
		    delete.setInt(1, num);
		    System.out.println("삭제완료");
		    delete.executeUpdate(); // 리턴값 1 0 
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
	}
	//글 수정하기
	public void update(BoardDTO dto) {
		try {
			con = getConnection();
			String sql_Update = "UPDATE board SET subject = ?, content = ? WHERE num = ?";
		    update = con.prepareStatement(sql_Update);
		    update.setString(1, dto.getSubject());
		    update.setString(2, dto.getContent());
		    update.setInt(3, dto.getNum());
		    update.executeUpdate();
		    
		}catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
		
	}
	
	//글 전체 개수 가져오기
	public int getBoardCount() {
		int count = 0;
		String sql_getBoardCount = "select count(*) as count from board";
		try {
			con = getConnection();
			select = con.prepareStatement(sql_getBoardCount);
			result = select.executeQuery();
			if(result.next()) {
				count = result.getInt("count");
			}
			
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		finally {
			dbClose();
		}
		return count;
	}
}
```

- BoardDTO (dao 에서 가져온 값을 저장 -> Model)
```java title:BoardDTO
package com.itwillbs.domain;

import java.sql.Timestamp;

public class BoardDTO {
	private int num;
	private String name;
	private String subject;
	private String content;
	private int readCount;
	private Timestamp date;
	
	public int getNum() {
		return num;
	}
	public void setNum(int num) {
		this.num = num;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	public int getReadCount() {
		return readCount;
	}
	public void setReadCount(int readCount) {
		this.readCount = readCount;
	}
	public Timestamp getDate() {
		return date;
	}
	public void setDate(Timestamp date) {
		this.date = date;
	}
}

```

---
##### Page 처리
```java title:pageDTO
package com.itwillbs.domain;


public class PageDTO {
	
	private int pageSize;
	private String pageNum;
	private int curentPage;
	
	private int startRow;
	private int endRow;
	
	private int startPage;
	private int endPage;
	
	private int pageBlock;
	private int pageCount;
	
	private int count;
	
	
	public int getStartPage() {
		return startPage;
	}
	public void setStartPage(int startPage) {
		this.startPage = startPage;
	}
	public int getEndPage() {
		return endPage;
	}
	public void setEndPage(int endPage) {
		this.endPage = endPage;
	}
	public int getPageBlock() {
		return pageBlock;
	}
	public void setPageBlock(int pageBlock) {
		this.pageBlock = pageBlock;
	}
	public int getPageCount() {
		return pageCount;
	}
	public void setPageCount(int pageCount) {
		this.pageCount = pageCount;
	}
	public int getPageSize() {
		return pageSize;
	}
	public void setPageSize(int pageSize) {
		this.pageSize = pageSize;
	}
	public String getPageNum() {
		return pageNum;
	}
	public void setPageNum(String pageNum) {
		this.pageNum = pageNum;
	}
	public int getCurentPage() {
		return curentPage;
	}
	public void setCurentPage(int curentPage) {
		this.curentPage = curentPage;
	}
	public int getStartRow() {
		return startRow;
	}
	public void setStartRow(int startRow) {
		this.startRow = startRow;
	}
	public int getEndRow() {
		return endRow;
	}
	public void setEndRow(int endRow) {
		this.endRow = endRow;
	}
	public int getCount() {
		return count;
	}
	public void setCount(int count) {
		this.count = count;
	}
}

```