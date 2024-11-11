[ 여러 명이 함께 Git 협업 ]
- Git에서 병렬 버전 관리는 브랜치(Branch)를 통해서 관리.

1. 프로젝트에서 A가 팀장을 맡았고, A의 GitHub 계정에 저장소를 생성하여 master를 관리하기로 함. A는 우선 자신의 GitHub 계정에 저장소를 생성하였다.
저장소 이름 : reservation_project

2. D 드라이브에 로컬 저장소를 생성(Project_Ex)

3. Project_Ex 폴더에 git 초기화 및 커밋, 푸시
- A는 메인페이지를 담당하여 introduction.jap 파일 생성.
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/jinsim2/reservation_project.git
git push -u origin main

4. A가 자신의 gitHub에 파일이 잘 업로드 되었는지 확인

5. 이제 프로젝트 조원인 B의 경우. B는 관리자 페이지를 담당하기로 했음. 우선 팀장 A가 만들고 업로드 한 github에 지금까지 작성된 버전을 받아옴.
이때 git clone 명령어를 사용
git clone https://github.com/jinsim2/reservation_project.git .

6. B는 관리자 페이지를 만들기로 했으므로 admin 폴더를 생성하고 adminMain.jsp를 생성

7. git add . -> git commit -m "B의 첫번째 커밋, admin 폴더와 adminMain.jsp 생성"

8. 커밋 결과를 github에 업로드 시 main에 올리면 안된다!!
main는 팀장인 A가 마지막에 합쳐서 프로젝트의 합본을 만들 때 사용해야 함.
B는 main 과는 별개의 공간인 곳에서 자신의 작업을 진행해야 함.

9. git checkout -b branch_B
=> B가 사용할 새로운 공간을 생성
=> main -> branch_B로 변경이 됨

10. B가 작업한 내용을 업로드
git push origin branch_B

11. B가 작업을 했고, branch_B에 push(업로드) 한 내용을 팀장 A에게 알리면, A는 main에 병합(합치기)하는 과정을 진행.
gitHub 페이지로 가보면 'branch_B had recent pushes...' 메시지가 보이고 오른쪽에  보이는 [Compare & pull request] 버튼 클릭

12. Add a description 에 내용을 넣고 [Create pull reqest] 버튼 클릭

13. 그러면 Pull requests에 1이 생겨나고 branch_B의 내용을 main에 보낼 수 있도록 허락을 구하는 중이라고 생각하면 됨.

14. 이제 다시 팀장 A가 github에 접속했다고 가정. Pull request(1) 을 보고 클릭. 팀원B가 작업을 하여 branch_B에서 main으로 보내온 것을 알 수 있음.

15. 코드에 별 문제가 없다고 가정을 하고 진행. [Merge pull request]을 클릭 -> [Confirm merge]까지 클릭하면 main에 합쳐지게 된다.

16. 상단탭의 '<>Code' 클릭

17. B가 작업한 브랜치 'branch_B'의 admin 폴더가 main 브랜치에도 보이게 됨.

18. 다른 팀원들이 작업을 각각 했다고 가정을 하면. 이제 각 팀원들은 수정되고 병합된 최종 프로젝트로 동기화를 시켜줘야 한다!

19. A가 작업 중이던 Project_Ex 폴더가 열린 Git Bash로 이동해서 git pull origin main 을 입력하여 최종 버전의 main을 동기화 시킴

20. 이제 팀원들은 작업을 하기 전(항상!) git pull origin main 을 입력해서 동기화를 시켜놓고 본인의 작업을 하고, 할당된 자신의 branch에 push하고 pull 하는 작업을 반복하면서 프로젝트를 완성시킴!!
==============================================
정리하면 협업 시 브랜치 사용
1) 협업자는 커밋을 올릴 브랜치를 생성 : git checkout -b 브랜치명
2) 자신이 만든 브랜치로 이동 : git checkout 브랜치명
3) 브랜치에 커밋을 올리고 : git push origin 브랜치명
4) 작업이 완료되면 브랜치를 합친다 : git merge main


