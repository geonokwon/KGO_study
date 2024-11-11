- 윈도우의 바로가기와 같은 개념
- 리눅스에서 systemctl enable 서비스명을 입력하면 심볼릭 링크가 만들어짐
- 예를 들어 systemctl enable mariadb를 입력하면
	- Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
	- 출력됨
	- symlink(심볼릭링크) 가 만들어짐
	- ls -l /bin 입력
	- lrwxrwxrwx. 1 root root 7  5월 24 15:20 /bin -> usr/bin
	- 심볼릭 링크를 통해서 파일을생성하면 i-node 번호가 다르게 생성됨
---
#### 하드링크와 심볼릭링크의 차이점
-  ***하드링크의 ***경우 링크파일의 온본을 삭제해도 하드링크 파일엔 아무런 영향이 없음
	- 디렉터리에 대한 링크를 만들 수 없음
- ***심볼릭링크***의 경우 링크파일을 원본을 삭제하면 심볼릭 링크 파일에 영향을 미침
  -> 그 이유는 심볼릭 링크 파일은 원본 파일의 i-node 번호를 참조하기 때문!
	- 디렉터리에 대한 링크를 만들 수 있음

