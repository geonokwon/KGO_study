[ 컴퓨터에 git bash 설치하기 ]
1. https://git-scm.com/downloads 접속

2. Download for Windows 클릭
- Standalone Installer 항목 중 64-bit Git for Windows Setup 클릭

3. 설치 파일 더블 클릭 -> Next , 변경 없이 계속 Next
- 설치 경로  C:\Program Files\Git

4. 설치가 완료되면 □View Release Notes 체크 해제하고 [Finish] 버튼 누름

5. 바탕화면 우클릭 -> 추가옵션표시 -> Open Git bash here 클릭

6. 화면이 뜨면 git 이라고 입력한 후 엔터

7. git --version 입력하면 버전 확인 가능

[ 로컬 저장소 ]
- 컴퓨터에 Git과 연결할 로컬저장소를 생성
- 로컬저장소는 실제로 Git을 통해 버전관리가 이루어질 컴퓨터 내의 폴더

1. D 드라이브 아래에 StudyGit 폴더 생성

2. StudyGit 폴더 안에 README.txt 텍스트 파일을 하나 생성.

3. README.txt 를 열어서 "아이티윌 부산교육센터 프로젝트" 라고 저장 후 닫음.

4. 폴더 안에서 마우스 우클릭 -> 팝업창에서 [Open Git Bash Here] 를 클릭

5. Git Bash 창이 열리면 git init 명령어를 입력
=> StudyGit 폴더에 .git 폴더 생성됨

6. 만약, 보이지 않으면 탐색기 창의 보기탭 -> 표시 -> 숨긴 항목 체크

7. 'git' 폴더에는 Git으로 생성한 버전들의 정보와 원격저장소의 주소 등이 들어있음. 그리고 .git 폴더를 로컬저장소라고 부름.

[ 첫 번째 커밋 만들기 ]
- README.txt 파일을 하나의 버전을 만듦
- Git에서 생성된 각 버전을 커밋(Commit)이라고 부름

1. 버전 관리를 위해서 내 정보를 등록해야 함.
각 버전을 누가 만들었는지 알아야 협업하기 편하기 때문
git config --global user.email "이메일주소"
git config --global user.name "git유저명"

2. 커밋에 추가할 파일 선택
git add README.txt
※ git add . => 모든 파일을 추가할 때

3. git status 를 입력하면 new file : README.txt 가 보임.
새로운 버전(커밋, Commit)을 생성하게 되면 추가될 파일이 보임.
※ git status 는 필수 명령어는 아님!

4. 상세 설명과 함께 커밋을 만듦
git commit -m "새 프로젝트 시작"
=> 1 file changed, 1 insertion(+) 메시지와 함께 커밋 성공!

5. StudyGit 폴더로 이동해서 README.txt 파일을 열어서 "아이티윌 부산교육센터 1강의 프로젝트"로 수정

6. git status 입력하면 modified : README.txt 가 보임
=> 앞에서 만든 커밋(첫번째 버전)에서부터 변경사항이 생김

7. 두 번째 커밋을 만듦
git add .
git commit -m "설명 업데이트"
=>  1 file changed, 1 insertion(+), 1 deletion(-) 메시지 보임

[ 커밋 이동 ]
1. 현재 README.txt 파일의 내용은 두 번째 커밋이고, "아이티윌 부산교육센터 1강의장 프로젝트" 의 내용을 가지고 있음. 첫번째 커밋으로 이동

git log
=> 지금까지 만든 커밋을 확인

2. 두 개의 커밋을 확인할 수 있음.
commit 5a61461~ : 설명 업데이트
commit 962fd62~ : 새 프로젝트 시작
되돌리려는 커밋을 첫 번째 커밋이고, 앞에서부터 7자리 아이디를 복사하고 checkout 명령어로 해당 커밋으로 되돌림

git chekcout 962fd62
명령어 입력 후 README.txt 를 열어보면 1강의장 단어가 없어진 것을 볼 수 있음

3. 두 번째 커밋으로 이동하기
git checkout -
=> 최신 커밋으로 이동
=> 다시 1강의장이 포함된 README.txt 를 볼 수 있음.

[ 원격저장소 ]
- 원격저장소(Remote Repository)는 프로젝트를 위한 공용 폴더 정도로 생각.

1. GitHub 접속 후 로그인, 우측 상단의 [+] 아이콘을 클릭한 후 [New repository] 선택

2. 이름에 적당한 이름을 입력하고(ex. TestRepository) [Create Repository] 클릭

3. https://github.com/jinsim2/TestRepository 의 경로가 생겨남
=> jinsim2 부분은 본인의 username.

[ 원격저장소에 커밋 올리기 ]
1. StudyGit 폴더의 Git Bash로 돌아옴
git remote add origin https://github.com/jinsim2/TestRepository.git

2. git remote -v
=> 생성한 원격저장소 주소 확인
※ 만약, 원격저장소 등록 시 오타나 다른 주소를 입력했다면 
git remote rm origin 명령어로 삭제한 후 다시 1번 과정을 통해 재등록!

3. 로컬저장소에 있는 커밋들을 push 명령어로 원격저장소에 올림
git push origin master 를 입력 GitHub의 로그인창이 뜸

4. Sign in with your browser 를 클릭해서 로그인을 진행
=> 로그인 성공하면 입력했던 명령어가 실행됨!

5. GitHub 페이지로 이동해서 새로고침(F5)을 하면 원격저장소의 내용이 변경되어 있음.
README.txt 파일이 보이고, 내용도 잘 보임.

6. StudyGit 폴더에 index.html 을 생성하고 원격저장소에 push
1) StudyGit 폴더에 index.html 만들기
2) git add .
3) git commit -m "세 번째 커밋(index.html)추가"
4) git push origin master

7. GitHub 페이지로 가서 새로고침(F5)을 하면 index.html 이 보임!

[ 원격저장소의 커밋을 로컬저장소에 내려받기 ]
- 원격저장소의 코드와 버전(커밋) 전체를 내 컴퓨터에 내려받는 것을 클론(clone)이라고 함.

1. D 드라이브에 StudyGit2 폴더를 생성

2. 생성한 StudyGit2 폴더 안에서 Open Git Bash Here 를 눌러 Git Bash 실행

3. GitHub 페이지에서 <>Code▼ 클릭 해서 URL 주소를 복사
git clone https://github.com/jinsim2/TestRepository.git .
=> 현재 폴더(StudyGit2)에 원격저장소를 내려받음
=> 마침표(.)를 넣지 않으면 TestRepository 폴더가 생기고 그 폴더 안에 파일이 내려받아짐

4. README.txt 파일을 수정하고 저장

5. 변경된 내용을 원격저장소에 푸시
git add .
git commit -m "개발자 목록 추가"
git push origin master

[ 원격저장소의 새로운 커밋을 로컬저장소에 갱신]
- StudyGit2 폴더에 있는 README.txt 내용과 StudyGit 폴더에 있는 README.txt 내용이 다름
- 현재 원격저장소에는 StudyGit2의 커밋이 반영되어 있음.

1. StudyGit 폴더의 Git Bash 이동

2. git pull origin master
=> pull은 원격 저장소에 새로운 커밋이 있다면 그걸 내 로컬저장소에 받아오는 명령어

3. 1 file changed, 2 insertions(+), 1 deletion(-) 메시지와 함께 README.txt 파일이 새 버전으로 갱신!

[ GUI 환경에서 버전 관리 - 소스트리 ]
1. https://www.sourcetreeapp.com/ 접속

2. [ Download for Windows ] 버튼 클릭

3. □ I agree to the Atlassian Software License Agreement and Privacy Policy. 체크 후 다운로드 진행

4. 설치 파일 더블 클릭 설치 시작

5. 설치 화면이 뜨면 Bitbucket 클릭 -> 로그인 화면이 뜸
아틀리시안 계정이 있다면 로그인을 하고 없으면 [계정만들기] 클릭

6. GitHub에 가입했을 때 사용한 이메일을 적고 가입을 누름
=> 입력한 이메일에 접속해서 [이메일 확인] 버튼을 클릭하면 이메일 주소 확인이 완료됨

7. Username을 입력하고 Continue 클릭

8. 설문조사는 [Skip]을 클릭해서 넘어가고 설치 화면으로 돌아온다. 아직 Login to Bitbucket 중이라면 [뒤로] 버튼을 누르고 다시 "Bitbucket" 버튼을 다시 누름.
현재 로그인된 비트버킷 계정에 소스트리에 접근할 수 있는 권한을 주는 창이 나오면 [Grant access] 버튼을 클릭해서 권한을 줌.

9. 소스트리 설치 화면으로 돌아가면 '등록완료' 페이지가 나오고, [다음] 클릭

10. □Mercuial(git과 비슷한 버전관리 툴) 체크 해제 [다음]

11. 소스트리에서 Git을 사용할 때 기본 계정으로 사용할 Git 계정을 설정
GitHub에서 가입했던 계정 정보를 입력하고 [다음]

12. SSH 키를 불러올 것인지 묻는 알림 창이 뜸 -> 아니오.

13. 소스트리 메인화면이 뜸.

[ 소스트리에서 GitHub 로그인하기]
- 소스트리 메인화면에서 원격저장소 탭인 [Remote] 탭을 클릭
- 기본적으로 Bitbucket이 호스팅 사이트가 되어 있음.
- GitHub를 Git 호스팅 사이트로 사용할 것이므로 기본 설정값을 변경

1. Bitbucket 계정에서 마우스 우클릭 -> 계정 편집

2. 호스팅 계정 편집 창이 뜨면 호스팅 서비스 : GitHub로 변경, 
OAuth 토큰 새로고침 버튼 클릭. 
만약 새로운 창이 뜬다면 'Authorize altlassian' 클릭 -> 패스워드 입력 후 Confirm

3. GitHub에 등록한 원격저장소 목록이 보임. 
만약, 목록이 보이지 않으면 새로고침 버튼을 클릭

[ 소스트리로 GIT 버전 관리하기 ]
1. 소스트리를 실행하고 [Add] 탭 클릭

2. [탐색] 버튼을 누르고, D 드라이브 아래에 StudyGit 폴더를 찾아 [폴더선택] 버튼을 클릭, 추가

3. StudyGit 로컬저장소에서 버전 관리를 할 수 있는 새로운 탭이 열림
History를 클릭하면 커밋들을 볼 수 있음.

[ 소스트리로 커밋 만들고 푸시하기 ]
1. StudyGit 폴더로 이동

2. 새파일 subject-list.md 파일 생성하고 적당한 내용을 입력
※ .md 확장자는 txt 확장자와 동일하게 텍스트 문서를 적기 위한 파일.
마크다운 문서라도 부름.

3. 소스트리로 이동하면 스테이지 올라가지 않은 파일 항목에 subject-list.md 파일이 보임. 

4. subject-list.md 파일 오른쪽에 있는 [+] 아이콘을 클릭
=> 스테이지 올라간 파일 위치로 이동

5. 왼쪽 WORKSPACE 아래의 '파일 상태'를 클릭 -> 아래쪽 입력창에 간단한 설명을 입력(ex. 과목리스트 추가) -> [커밋] 버튼을 클릭.

6. History 에서 확인하면 과목리스트 추가 항목이 보임.
=> 과목리스트추가에 'master' , 개발자 목록 추가에 'origin/master' 네모 박스가 있음

[ 소스트리에서 커밋을 원격저장소에 푸시]
- master : 로컬저장소, origin/master : 원격저장소

1. 새로 만든 커밋을 원격저장소에 업로드
소스트리 상단의 [Push] 버튼을 클릭하고, [master] 옆의 체크박스 체크
push 버튼을 클릭
=> git push origin master 명령와 동일

2. Select an account 창이 나오면 본인의 github 계정을 선택, Continue

3. 과목리스트 추가 설명이 있는 버전(Commit)에 master와 origin/master가 존재

4. GitHub 사이트로 이동해서 새로고침(F5)을 하면 subject-list.md 파일이 보임!

