- http://www.servlets.com/cos/ 
-  cos 파일 다운로드
-  라이브러리 폴더 -> cos.jar 파일만 추출
-  WEB-INF -> Lib 폴더에 넣어준다!
-  MultipartRequest 클래스 사용

- MultipartRequest 클래스 -> 객체생성(인스턴스화) -> 기억장소 할당, 초기값을 할당
- 객체생성 => 파일업로드 -> 파리미터값 가져오기
- `MultipartRequest multi = new MultipartRequest();
- 파라미터 값 5
	1. request 
	2. saveDirectory : 파일 업로드 폴더
	3. maxPostSize : 파일 최대 크기
	4. encoding : 한글처리 (인코딩 방식)
	5. policy : 파일이름 중복처리
- `MultipartRequest multi = new MultipartRequest(request, uploadPath, maxSize, "utf-8", new DefaultFileRenamePolicy());`
---
- 톰캣 10 버전이 -> cos.jar 파일을 jakartaee -> 마이그레이션 해야함!
	-  https://tomcat.apache.org/download-migration.cgi
	-  Tomcat Migration Tool for Jakarta EE Software Downloads
	-  shaded jar (pgp, sha512)
	-  jakartaee-migration-1.0.8-shaded.jar 다운로드
	-  cos.jar 다운로드 => cos.jar Jakarta EE 버전으로 변경
	-  cmd
	-  d:
	-  java -jar jakartaee-migration-1.0.8-shaded.jar cos.jar cosj.jar
	-  WEB-INF - lib - cosj.jar 넣기
