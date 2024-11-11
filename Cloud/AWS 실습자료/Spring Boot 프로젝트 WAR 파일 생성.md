# Spring Boot 프로젝트 WAR 파일 생성
1. Help -> Eclipse Marketplace
2. search -> find : web -> Eclipse Enterprise Java and Web Developer Tools 3.x installed
3. 기본 선택 confirm -> finish -> 창이 뜨면 Select all 선택 후 오른쪽 하단의 Trust Selected 선택 -> sts4 재시작
4. 프로젝트 우클릭 -> Properties -> Project Facets -> Dynamic Web Module 체크 Apply and Close 선택
5. 프로젝트 우클릭 -> export -> web -> war 선택
---

1. build.gradle 파일 수정
```
plugins {
		...
		id 'war' 추가
	}
	dependencies {
	....
	implementation 'org.springframework.boot:spring-boot-starter-web'
	providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
	}
```
	
2. src/test/java 아래의 com.itwillbs 패키지 아래에 Application.java 클래스 생성
``` java
package com.itwillbs;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

public class Application extends SpringBootServletInitializer {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(Application.class);
	}

}
```
---
3. cmd 실행
프로젝트 폴더로 이동(workspace 프로젝트명)
ex) cd d:\Shared\JSP\workspace_sts4\bootProject

4. gradlew clean build
=> .war 파일을 build/libs 폴더(디렉터리) 아래에 생성

5. war 파일 확인
프로젝트명-0.0.1-SNAPSHOP.war 형태로 만들어짐

6. 만들어진 war 파일 수정
F2를 눌러 이름을 프로젝트명.war로 변경한다.

7. war 파일을 MobaXterm 등의 툴을 사용해서 톰캣 서버의 webapps로 복사한다.
---