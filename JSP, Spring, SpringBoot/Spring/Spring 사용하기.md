# Spring 사용하기
- 자바로 만든 프레임 워크
- 전자정부표준 프레임워크
---
- 장점 
	- 코드가 간결(경량 프레임워크)
- 개념
	1. MVC
	2. 객체생성(수정최소화, 반복 줄여줌) : 의존관계주입(DI), 제어의역전(IoC)
	3. JDBC(마이바티스)
	4. 파일업로드, ajax(rest서비스)
	5. AOP
---
##### 설치(Eclipse)
- 톰캣 9버전 설치 ( [32-bit/64-bit Windows Service Installer](https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.exe) ([pgp](https://downloads.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.exe.asc), [sha512](https://downloads.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.exe.sha512)))
---
1) 이클립스 마켓플레이스 Spring 검색
	- Spring tool 인스톨
2) Spring Tool 다운받아 압축풀어서 사용
	- 3.x.x 버전 다운로드 https://github.com/spring-attic/toolsuite-distribution/wiki/Spring-Tool-Suite-3 
	- c 드라이버 압축풀고
	- sts-bundle 폴더 생성
	- STS.exe 파일 -> 바로가기 만들기
	- d 드라이브  workspace_sts 폴더 생성
	- sts 파일 실행후 workspace 폴더 지정후 실행
3) 스프링 프로젝트 생성(Spring Lagacy Project)
	- Spring MVC project 선택
	- 패키지 com.도매인.myweb
---
### Spring 이용하여 게시판 만들기
- @Controller <- 지정하기
- @RequestMapping(value = "/가상주소" mathod = "Request)
- 자동으로  되는 조건(MemberDTO memberDTO) 
	- insert.jsp 태그이름 id, pass, name
	- MemberDTO 멤버변수 이름 id, pass, name
	- 서로 일치가 되어야 자동으로 값이 들어가짐
---
#### 스프링 객체 생성
- 장점 : 자바파일 수정없이 스프링파일 한번만 수정하면 모든 자바에 사용한곳은 수정됨, 코드 간결

### Spring version 5.0 (JDK 11 ver)
- 앞선 3.0 버전에서 @Controller 에서 @RequestMapping(value = "/member/insert", method = RequestMethod.GET) 하였던것을 @GetMapping ("값") 으로 바꿀수 있고
- MemberService 객채를 @Service로 지정후 @Autowired 걸어서 객채생성을 스프링에서 하도록 변경

```java title:Java
package com.itwillbs.controller;

import javax.inject.Inject;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import com.itwillbs.domain.MemberDTO;
import com.itwillbs.service.MemberService;

@Controller
public class MemberController {
	
	
	//멤버변수 MemberService 객체생성 => @Service MemberService 찾아서 객채생성 => 변수전달
	@Autowired
	private MemberService memberService;
	
	@GetMapping("/member/login")
	public String login() {
		System.out.println("MemberController login()<<");
		
		return "0.member/Login";
	}
	
	// @RequestMapping(value = "/member/insert", method = RequestMethod.GET)
	@GetMapping("/member/insert")
	public String insert() {
		System.out.println("MemberController insert()<<");
		
		return "0.member/insert";
	}
	
	@PostMapping("member/insertPro")
	public String insertPro(MemberDTO memberDTO) {
		System.out.println("MemberController insertPro()<<");
		//회원가입 처리
		memberService.insertMember(memberDTO);
			
		return "redirect:/member/login";
	}
	
}

```

- MemberService
```java title:Java
package com.itwillbs.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.itwillbs.dao.MemberDAO;
import com.itwillbs.domain.MemberDTO;

@Service
public class MemberService {
	
	@Autowired
	private MemberDAO memberDAO;
	
	public void insertMember(MemberDTO memberDTO) {
		System.out.println("MemberService insert()<<");
		memberDTO.showInfo();
		memberDAO.insertMember(memberDTO);	
	}
	
}

```

