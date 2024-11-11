#  AWS Auto Scaling 
- 기준값에 따라 서버의 수를 자동! 으로 늘였다 줄였다 하는 서비스
- 사람이 아닌 AWS에서 자동으로 추가/삭제

AWS Auto Scaling 절차
1. EC2 인스턴스 스냅샷 생성
2. 시작 템플릿 생성
3. Auto Scaling 생성

[1. EC2 인스턴스 스냅샷 생성]
1. EC2 대시보드 인스턴스 -> 인스턴스 -> execise-instance1 이 중지됨 상태인지 확인

2. 우클릭 -> 이미지 및 탬플릿 -> 이미지 생성
- 이미지 이름 : exercise-image1
- 나머지 설정은 기본값으로 설정
- 이름을 넣었다면 [이미지 생성] 버튼 클릭

3. 상단에 '현재 AMI 생성 중' 메시지 표시
왼쪽 항목 이미지 -> AMI 클릭
AMI 이름 : exercise-image1, 상태 : 사용가능(avaiable) 인지 확인

※ 스냅샷은 프리티어 요금제를 사용하면 1G만큼의 용량을 무료로 사용할 수 있지만, 유지 비용은 따로 발생!

---

[2. 시작 템플릿 생성]
1. 인스턴스 -> 시작 템플릿 -> [시작 템플릿 생성] 버튼 클릭

2. 시작 템플릿 생성창
- 시작 템플릿 이름 : exercise-launch-template
- 템플릿 버전 설명 : initial version
- 애플리케이션 및 OS 이미지
  내 AMI -> 내 소유 -> exercise-image1
- 인스턴스 유형 : t2.micro
- 키 페어(로그인) : 기존 것
- 네트워크 설정 : 기존 보안 그룹 선택(ssh, web)
- 스토리지 볼륨 : 기본값
설정을 다 했으면 [시작 템플릿 생성] 버튼 클릭

3. 시작 템플릿 보고 클릭

---

[ 3. Auto Scaling 생성 ]
1. 왼쪽 메뉴에서 스크롤을 내려 Auto Scaling -> Auto Scaling 그룹

2. [Auto Scaling 그룹 생성] 버튼 클릭

3. Auto Scaling 그룹 생성 화면
- 총 7단계로 구성
- 1단계 시작 템플릿 또는 구성 선택
=> Auto Scaling 그룹 이름 : EXERCISE-GROUP
=> 시작 템플릿 : exercise-launch-template
'다음' 클릭

- 2단계 인스턴스 시작 옵션 선택
=> VPC : 기본값 사용
=> 가용 영역 및 서브넷 : ap-northeast-2a, ap-northeast-2c
' 다음' 클릭

- 3단계 고급 옵션 구성
=> 로드밸런싱 : 로드밸런서 없음
※ 로드밸런서는 다음 내용으로 나옴!
=> 3단계는 설정 없이 '다음' 클릭

- 4단계 그룹 크기 및 크기 조정 구성
=> 그룹 크기 : 원하는 용량 1
=> 크기 조정 : 원하는 최소용량 1, 원하는 최대용량 2
=> 원하는 용량은 목표로 하는 인스턴스의 수, 최소 용량은 최소로 유지할 인스턴스의 수, 
     최대 용량은 크기 조정 정책에 따라 최대로 늘일 수 있는 인스턴스 개수

◎ 대상 추적 크기 조정 선택 
    크기 조정 정책 이름 : Scale Group Size
    지표 유형 : 평균 CPU 사용률(기본값)
    대상 값 : 50 -> 80 으로 변경
'다음' 클릭

- 5단계 : 알림 추가
 => 설정 없이 '다음' 클릭

- 6단계 : 태그 추가
 => [태그 추가] 클릭
 => 키 : Name, 값 : exercise-group
입력 후 '다음' 클릭

- 7단계 : 검토
=> 정보 확인 후 [Auto Scaling 그룹 생성] 버튼 클릭

---
EC2 인스턴스 -> 인스턴스 선택하면 exerice-group 이라는 이름의 인스턴스가 동작 중!
=> Auto Scaling group의 원하는 용량이 1이기 때문

※ 만약, exercise-group 인스턴스가 동작하지 않게 하려면 Auto Scaling 그룹의 
   원하는 용량을 0, 최소용량 : 0, 최대용량: 0 으로 설정!

---
[ Auto Scaling 동작 확인]
- cpu의 사용량을 인위적으로 높이는 stress 라는 패키지를 exercise-group 인스턴스에 설치
- stress 프로그램 동작
- cpu 사용량이 80% 가 넘어서면 최대 용량 2 설정값에 의해서 exercise-group 인스턴스가 하나 더 생성!

원격 접속
exercise-group 인스턴스의 퍼블릭 IPv4 주소를 설정하여 접속

sudo amazon-linux-extras install epel -y
=> 외부 저장소에서 패키지를 받을 수 있도록 설정

sudo yum install stress -y
=> cpu 사용량을 강제로 높이는 stress 패키지를 설치

stress --cpu 1 --timeout 600
=> cpu 하나를 600초(10분) 동안 100% 만듦
=> 10분을 설정하는 이유는 사용량 지표를 5분에 한 번씩 모니터링 서버로 전송하기 때문
=> Auto Scaling 그룹 생성 시 [세부 모니터링 활성화] 옵션을 활성화시키면 인스턴스가 
     1분마다 지표를 보냄. 단, 이 설정을 추가 비용이 발생!!

stress 프로그램(패키지) 동작 후 5 ~ 10분 정도 기다리면 exercise-group 인스턴스가 하나 더 생겨남!
=> 그 이유는 모니터링 시점(5분에 한 번)에 stress 프로그램에 의해 cpu가 80% 넘어선 100% 유지되기 때문!

10분 동안 동작하던 stress 프로그램이 종료되면, 다음 번 모니터링 시점에 exercise-group 인스턴스 하나를 종료(삭제) 시킴

★ 위의 과정이 모두 사람이 직접 하는 것이 아니라! 자동으로 이루어지는 과정!!
=> Auto Scaling 






   






 




