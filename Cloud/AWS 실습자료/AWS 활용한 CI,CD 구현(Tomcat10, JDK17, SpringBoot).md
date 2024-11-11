#  AWS 활용한 CI/CD 구현(Tomcat10, JDK17, SpringBoot)
- GitHub, CodeBuild, CodeDeploy, CodePipeline을 활용한 CI/CD
- GitHub에 코드를 푸시하면 CodePipeline이 이를 감지하고 CodeBuild를 실행시켜 빌드를 함.
  CodeDeploy가 EC2상의 Agent를 실행시켜 변경된 내용을 배포.

1. GitHub에 로그인을 하고 저장소(Repository)를 생성
- Repository name : CodePipeline-sample-springboot
- Description : AWS CodePipeline Sample Repository
나머지는 기본값으로 두고 [create repository] 를 클릭

2. 컴퓨터(윈도우)에 로컬 저장소 D: 에 bootProject 폴더를 생성
폴더 내에 만들어진 스프링부트 프로젝트 내용을 복사.
메모장으로 README.md 파일을 하나 생성하고 적당한 내용을 입력.
git bash 를 통해 초기화를 진행.
git init
git add .
git commit -m "first commit"
git branch -M main
git config --global user.email "이메일주소"
git config --global user.name "github 아이디"
git remote add origin https://github.com/jinsim2/CodePipeline-sample-springboot.git
git push -u origin main

3. AWS CodeBuild 페이지로 이동
프로젝트 생성 클릭
- 프로젝트 이름 : codepipeline-build-sample
- 소스공급자 :  GitHub
- 리포지토리 : https://github.com/jinsim2/CodePipeline-sample-springboot
- 역할 이름 : codebuild-codepipeline-build-sample-service-role
- Buildspec -> buildspec 파일 사용 선택
[ 빌드 프로젝트 생성 ]
=> CodeBuildBasePolicy-codepipeline-build-sample-ap-northeast-2 정책 생겨남

4. VS Code 실행 -> 왼쪽의 탐색기 항목에서 "Open Folder" 클릭 -> 2번에서 생성한 bootProject 선택

5.  build.gradle 파일에서 의존성 및 plugin 설정
bootproject 폴더 내의 build.gradle 파일 열기
===============================
// 플러그인 설정
// 필수 Gradle 플러그인을 정의한다.
// 스프링 부트 애플리케이션을 빌드하고 실행하는 데 필수. 
//  WAR 파일 생성을 위해 'war' 플러그인도 추가한다.
plugins {
	id 'java'
	id 'war'
	id 'org.springframework.boot' version '3.3.4'
	id 'io.spring.dependency-management' version '1.1.6'
}

group = 'com.itwillbs'
version = '0.0.1-SNAPSHOT'

// Java 버전 설정
// JDK 17로 설정하여 호환성을 확보한다.
java {
	toolchain {
		languageVersion = JavaLanguageVersion.of(17)
	}
}

// 저장소 설정
// Maven 중앙 저장소를 사용한다.
repositories {
	mavenCentral()
}

// 의존성 설정
// Spring Boot를 사용하지만, 내장 Tomcat은 제외하고 WAR 파일로 패키징하기 위해
// 'providedRuntime' 의존성을 추가한다.
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'	testImplementation 'org.springframework.boot:spring-boot-starter-test'	
}

// 테스트 설정
// JUnit 플랫폼을 사용하여 Java 테스트를 구성한다.
tasks.named('test') {
	useJUnitPlatform()
}

// WAR 파일 설정
// 기본적으로 Spring Boot는 JAR 파일을 생성하지만
// bootWar 태스크를 통해 WAR 파일을 구성할 수 있다. WAR 파일 이름을 설정한다.
bootWar {
	archiveBaseName = 'myapp'
	archiveVerson = '0.1.0'
}

---

6. AWS CodeBuild가 빌드 시 참고하는 buildspec.yml 만듦
---
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto17
    commands:
      - yum update -y
      - echo Installing SDKMAN and Gradle
      - curl -s "https://get.sdkman.io" | bash
      - source "$HOME/.sdkman/bin/sdkman-init.sh" && sdk install gradle  # Gradle 설치
  pre_build:
    commands:
      - echo Build started on 'date'
      - chmod +x gradlew  # ./gradlew 파일에 실행 권한 추가
  build:
    commands:
      - echo Building the Spring Boot application
      - source "$HOME/.sdkman/bin/sdkman-init.sh" && ./gradlew clean bootWar  # 빌드 명령
      - ls -l build/libs/
      - mv build/libs/\*.war myapp.war
  post_build:
    commands:
      - echo Build and packaging completed on 'date'
artifacts:
  files:
    - myapp.war
    - appspec.yml
    - scripts/*

---

7. codedeploy 가 사용할 appspec.yml 파일을 생성
---
version: 0.0
os: linux
files:
  - source: myapp.war
    destination: /usr/local/tomcat/webapps/
hooks:
  BeforeInstall:
    - location: scripts/stop_tomcat.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/start_tomcat.sh
      timeout: 300
      runas: root

---

8. 로컬저장소(bootproject) 폴더 안에 scripts 라는 이름의 새폴더 생성 
scripts 폴더 내에 start_tomcat.sh, stop_tomcat.sh 새 파일 생성
===============================
[ stop_tomcat.sh ]
#!/bin/bash
sudo systemctl stop tomcat10

[ start_tomcat.sh ]
#!/bin/bash
\# 기존 WAR 파일 삭제
rm -f /usr/local/tomcat/webapps/ROOT.war

\# 새로운 WAR 파일 이동
mv /usr/local/tomcat/webapps/myapp.war /usr/local/tomcat/webapps/ROOT.war

\# Tomcat 재시작 (시스템에 따라 명령어가 다를 수 있음)
sudo systemctl restart tomcat10

---

9. git bash 창에서 스크립트 파일에 실행권한(x)을 추가
chmod +x scripts/start_tomcat.sh
chmod +x scripts/stop_tomcat.sh

10. buildspec.yml, appspec.yml, scripts/start_tomcat.sh, scripts/stop_tomcat.sh, 수정한 build.gradle을 main 브랜치 push
git add .
git commit -m "add buildspec, appspec, script files"
git push origin main

11. GitHub 사이트로 이동하여 main 브랜치에 파일이 정상적으로 잘 업로드 되었는지 확인

12. 이제 EC2 톰캣 서버를 생성
=> 1101 - [ AWS 프로젝트 톰캣 배포 서버 ] 파일 참고

13. EC2 톰캣 서버가 CodeDeploy로부터 배포를 받기 위해 EC2 톰캣 서버에 Agent를 설치
sudo yum -y update
sudo yum install -y ruby
cd /home/ec2-user
sudo curl -O https://aws-codedeploy-ap-northeast-2.s3.amazonaws.com/latest/install
sudo chmod +x ./install
sudo ./install auto

14. CodeDeploy 역할, 정책 설정
IAM 검색 -> 페이지 이동 -> 왼쪽의 '역할' -> 역할 생성
- 신뢰할 수 있는 엔터티 유형 : AWS 서비스
- 사용 사례 : CodeDeploy, 사용 사례 선택 : CodeDeploy
=> 권한 정책에 'AWSCodeDeployRole' 정책 이름이 추가되어 있음 [다음] 클릭
- 역할 이름 : CodeDeploy-ServiceRole
[역할 생성] 버튼 클릭

15. IAM 정책 -> 정책 생성 -> JSON탭 클릭, 정책 편집기에 아래의 내용을 붙여넣기
-----------------------------------------
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": [
			        "s3:List*",
			        "s3:Get*"
			 ],
			"Resource": "\*"
		}
	]
}

---------------------------------------------------
[다음] 클릭
정책 이름 : CodeDeployEC2-Permissions 입력 -> [정책 생성]

16. 생성된 정책을 역할로 연결하기 위해 [역할 생성] 버튼 클릭
- 신뢰할 수 있는 엔터티 유형 : AWS 서비스
- 사용 사례 : EC2
검색창에 codedeploy 입력 후 정책 리스트에서 'CodeDeployEC2-Permissions' 체크 후 [다음] 클릭
- 역할 이름 : CodeDeploy-Permissions 입력하고 [역할 생성] 버튼 클릭

17. CodeDeploy 페이지로 이동 -> 애플리케이션 생성

18. 애플리케이션 이름에 devops-codedeploy-sample 입력하고 컴퓨팅 플랫폼은 EC2/온프레미스 선택, [애플리케이션] 생성 버튼 클릭

19. 생성된 devops-codedeploy-sample 클릭 -> '배포그룹 생성' 버튼 클릭
- 배포 그룹 이름 : devops-codedeploy-sampledg
- 서비스 역할 : CodeDeploy-ServiceRole
- 배포 유형 : 현재 위치
- 환경 구성 : Amazon EC2 인스턴스 체크 -> Name, ApacheTomcat
- 로드배런싱 : 로드 밸런싱 활성화 체크 해제
[ 배포 그룹 생성] 버튼 클릭

20. EC2 인스턴스 대시보드로 이동하여 ApacheTomcat 인스턴스에서 우클릭 -> 보안 -> IAM 역할 수정 -> CodeDeploy-Permissions 선택
=> ApacheTomcat 인스턴스가 S3에 접근해서 파일을 받아오는 권한이 부여됨
=> CodeDeploy-Permissions 역할에는 S3와 관련된 정책이 설정되어 있음.

21. CodePipeline으로 이동 -> 파이프라인 생성 버튼 클릭
Build custom pipeline 선택 [다음] 클릭
- 파이프라인 이름 : devops-codepipeline-sample
- 소스 공급자 : GitHub(버전2)
- 연결 : devops-sample
만약, 항목이 나타나지 않으면 GitHub에 연결 -> 연결 이름 입력, GitHub에 연결 -> 새앱 설치 -> 모든 리포지토리 또는 특정 리포지토리 연결 
- 리포지토리 이름 : jinsim2/CodePipeline-sample-springboot
- 기본 브랜치 : main
- 트리거 항목의 브랜치 포함 : main 입력
[다음] 클릭

빌드 - 선택사항
Other build providers 선택 -> AWS CodeBuild -> codepipeline-build-sample
[다음]

배포 - 선택 사항
배포 공급자 : AWS CodeDeploy
애플리케이션 이름 : devops-codedeploy-sample
배포 그룹 : devops-codedeploy-sampledg
[다음]

전체 내용을 검토한 후 [파이프라인 생성] 버튼 클릭

22. 모든 과정(소스, 빌드, 배포)가 성공이 되었다면 EC2 톰캣 서버의 /usr/local/tomcat/webapps에 ROOT, ROOT.war 생성됨.

23. 파이프라인이 제대로 동작해서 빌드와 배포가 잘 되는 것을 확인했다면 main 브랜치의 변경사항을 push 하면 빌드와 배포가 자동으로 이루어지는지 확인

24. develop 브랜치를 생성, 앞으로 작업은 develop 브랜치에서 함
git checkout -b develop
git push --set-upstream origin develop

25. 수정 한 내용이 push를 하면 develop 브랜치에 저장.

26. develop 브랜치를 main 브랜치에 머지(합치기)를 하면 CodePipeline이 감지해서 빌드, 배포를 자동으로 수행.

---

실습 종료 후 자원 삭제
1) 파이프라인 삭제
2) CodeDeploy 애플리케이션 삭제
3) CodeBuild 프로젝트 삭제
4) S3 범용 버킷 삭제
5) EC2 인스턴스 종료
6) IAM 역할 삭제
- AWSCodePipeline...., ASWCodeDeploy... 로 시작 역할 삭제
7) IAM 정책 삭제
- pipeline 검색해서 삭제, DelopyEC2-Permissions 정책 삭제
8) git repository 삭제







