- web.xml 파일 설정하기
- servlet-mapping
```xml title:web.xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://jakarta.ee/xml/ns/jakartaee" xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd" id="WebApp_ID" version="6.0">
  <display-name>MVCProject</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.jsp</welcome-file>
    <welcome-file>default.htm</welcome-file>
  </welcome-file-list>
  
  <servlet-mapping>
    <servlet-name>memberController</servlet-name>
    <url-pattern>*.me</url-pattern>
  </servlet-mapping>
  
  <servlet>
    <servlet-name>memberController</servlet-name>
    <servlet-class>com.itwillbs.controller.MemberController</servlet-class>
  </servlet>
  
</web-app>
```

- com.itwillbs.controller 패키지 않에 MemberController 클래스 파일 생성후 servlet 을 상속받고
- HttpServlet 의 doGet(), doPost() 메서드를 재정의 하기 위해 메서드 오버라이딩
- doGet(), doPost() 메서드를 통합하여 doProcess() 메서드로 받고
- request.getServletPath() 를 통해서 Path 값을 받아오고
- Path 값을 가지고 일치하는 패스가 들어왔을때 주소는 .me를 유지하면서 페이지 이동시키는 RequestDispatcher 사용하여 이동한다
- `RequestDispatcher dispatcher_main = request.getRequestDispatcher("0.member/Main.jsp");`
- `dispatcher_main.forward(request, rseponse);`

```java title:Java(controller)
package com.itwillbs.controller;

import java.io.IOException;

import org.apache.catalina.connector.Response;

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
			break;
				
			case "/updatePro.me" : System.out.println("/updatePro.me 가상주소 비교 일치");
				memberService.update(request);
				System.out.println("수정완료");
				rseponse.sendRedirect("info.me");
			break;
				
			case "/memberList.me" : System.out.println("/memberList.me 가상주소 비교 일치");
				HttpSession session1 = request.getSession();
				if(session1.getAttribute("id").equals("root")) {
					memberService.memberList(request);
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

- Service 클래스 만들어서 jsp 에서 Pro 역활을 하던 jsp 수행을 java 메서드로 뽑아서 쓸수있도록 만들어 준다
```java title:Java(Service)
package com.itwillbs.service;

import java.sql.Timestamp;

import com.itwillbs.dao.MemberDAO;
import com.itwillbs.domain.MemberDTO;

import jakarta.servlet.http.HttpServletRequest;

public class MemberService {
	
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
		
	    MemberDAO dao = new MemberDAO();
	    dao.insertMember(dto);
	}
	//로그인체크(LoginPro.jsp에 해당되는 내용)
	public boolean loginCheck(HttpServletRequest request) {
		System.out.println("Service loginCheck() <<");
		boolean isCheck = false;
		String id = request.getParameter("id");
	    String pass = request.getParameter("pass");
	    
	    MemberDAO dao = new MemberDAO();
	    
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
	public HttpServletRequest info(HttpServletRequest request) {
		HttpSession session = request.getSession();
		MemberDAO dao = new MemberDAO();
		MemberDTO dto = dao.getMember((String)session.getAttribute("id"));
		request.setAttribute("dto", dto);
		System.out.println(request.getAttribute("dto"));
		return request;
	}
	//회원삭제(delete.jsp에 해당되는 내용)
	public void delete(HttpServletRequest request) {
		String id = request.getParameter("id");
		MemberDAO dao = new MemberDAO();
		dao.deleteMember(id);
	}
	//회원정보수정(updatePro.jsp에 해당되는 내용)
	public void update(HttpServletRequest request) {
	    MemberDAO dao = new MemberDAO();
	    MemberDTO dto = new MemberDTO();
	    dto.setId(request.getParameter("id"));
	    dto.setPass(request.getParameter("pass"));
	    dto.setName(request.getParameter("name"));
	    dto.setDate(new Timestamp(System.currentTimeMillis()));
	    dao.updateMember(dto);
	}
	//회원리스트(list.jsp에 해당되는 내용)
	public void memberList(HttpServletRequest request) {	
		MemberDAO dao = new MemberDAO();
		ArrayList<MemberDTO> resultDTO = dao.getMemberList();
		request.setAttribute("arrayList", resultDTO);
	}

}

```