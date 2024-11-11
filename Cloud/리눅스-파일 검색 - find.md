- find 명령어는 특정 조건을 통해 결과를 출력
- 기본적으로 옵션(Option)과 행동(Action)의 두 가지 요소
- find \[path] \[expression] 의 형태로 사용하여 \[expression] 부분에 옵션과 행동이 표현 된다
- 표현의미
- +,- 의미 => +n(n보다 큰 값), -n(n보다 작은 값), n(정확히 n값)
- 파일종류 ->b(block device file), c(caracter device file), d(directory), p(named pipe), f(regular file), l(symbolic link), s(socket)
-  연산자 => !수식 (- not 수식) : 수식이 거짓일 때 참이 됨
- 수식1 수식2 : 수식1 and 수식2
- 수식1 -o 수식2 : 수식1 or 수식2

< 옵션 >
- -name : 해당 파일의 이름을 찾음
- -user : 해당 user의 파일이나 디렉터리 찾음
- -group : 해당 group의 파일이나 디렉터리 찾음
- -perm : 선택 퍼미션을 찾음
- -type : 해당 파일 유형을 찾음
- -atime n : n일 이전에 엑세스한 파일을 찾음
- -ctime n : 파일이 변경된 날짜로 찾음
- -mtime n : 파일이 수정된 날짜로 찾음
- -size n : n문자 길이의 파일을 의미
- -inum n : i-node 번호가 n인 파일을 찾음
- 
< 행동(Action) >
- -exec : 찾은 파일등과 연계해 명령을 내릴 수 있음. 이 옵션을 사용하면 \; 으로 끝나야 함
- -ok : 확인 메시지를 출력하는 것을 제외하고는 -exec 와 같음
- -print : 파일의 절대경로 출력
- -print() : 파일명의 절대경로 끝에 null 문자를 합해서 출력
- -prune : 현재 디렉터리 아래로는 내려가지 않음
- -fprint file : 출력을 파일에 저장
- /dev/null : 맨 뒤에 이 옵션을 사용하면 접근 권한이 없는 파일이나, 디렉터리에 접근 시 나오는 리스트는 화면에 출력하지 않음
- -ls : 맨 뒤에 이 옵션을 사용하면 상세출력하는 'ls -l' 와 같음\

##  find 명령어 활용 
find -name '\*.txt' 
=> 현재 위치해 있는 디렉터리(폴더)에서 하위 디렉터리까지 확장자가 .txt인 파일들을 검색

find / -name '\*.txt'
=> 루트 디렉터리에서부터 하위디렉터리까지 모두 확장자가 .txt인 파일들을 검사

find -name 'etc' -type d 
=> 현재 위치해 있는 디렉터리에서 'etc' 디렉터리를 찾음

find -name '\*.txt' -ls 
=> 현재 위치한 디렉터리에서부터 하위디렉터리까지 확장자가 .txt인 파일을 찾고 상세내용(ls -l) 을 화면에 출력

find -user itwill
=> 현재 위치한 디렉터리에서부터 하위디렉터리까지 소유자가 itwill인 파일들을 찾음

find / -user itwill
=> path가 '/(루트 디렉터리)'로 설정되어 리눅스 내의 모든 파일이 검색 대상이 됨
=> 소유자가 itwill인 파일 및 디렉터리들을 출력

find /home/itwill -perm 600
=> /home/itwill 디렉터리(폴더) 아래에 있는 파일들 중 퍼미션이 600(rw-------)인 파일들만 화면에 출력

find /bin -type l 
=> /bin 디렉터리 아래에 링크 파일을 찾음

find -type d -name '.c*'
=> 현재 디렉터리 아래에 있는 모든 하위 디렉터리를 찾고 그 중에서 확장자가 .c로 시작하는 디렉터리(폴더) 찾음

find -type f -name 't*.txt' -exec rm {} \\;
=> 1) find -type f -name 't*.txt' 검색
    2) 검색된 파일들을 {} 안에 적용(넣음)
    3) -exec 옵션에 의해서 rm 명령어가 실행, 그 명령어 끝을 '\\;' 으로 표현

Access 변경
=> cat 명령어를 사용하면 시간이 변경됨

Modify
=> vi 명령어를 통해 파일 내용이 수정되면 변경됨

Change
=> mv 명령어를 통해 파일 이름이 바뀌면 변경됨

 find -ctime -1 -type f -name 't*.txt'
=> 리눅스 현재 시간을 기준으로 해서 하루 전에 변경된 파일을 찾음
=> -ctime -mtime -atime 옵션은 뒤에 \[+, -] n 의 형태로 구성됨 , \[+, -] 없으면 정확히 n일, +n이면 초과, -n이면 n일 미만인 파일들을 검색

find ~ -size 0k -exec ls -l {} \\;
=> 크기가 0인 파일 을 찾아서 ls -l 명령어를 적용해서 화면에 출력