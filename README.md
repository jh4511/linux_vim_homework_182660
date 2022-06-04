# linux_vim_homework_182660
리눅스 명령어 중 top, ps, jobs, kill 명령어와 vim 에디터에서 매크로 사용방법(q, @)에 대해 정리한 글임.
## 리눅스 명령어
### 1) top 명령어
1) __top 명령어란?__
+ 리눅스 시스템 운용상황을 실시간으로 전반적인 상황을 모니터링하거나 프로세스 관리를 할 수 있다.
  + 윈도우의 작업관리자랑 비슷하다고 생각하면 된다. 
+ top 명령어는 시스템의 프로세스와 메모리 사용상태를 5초의 간격으로 업데이트하여 화면에 출력
  + (사람마다 3초~5초의 간격으로 갈리는 것 같다. 기본 3초라고 할 때도 있음)
+ top 명령어로 확인하는 대표적인 것
+ 서버평균부하율
+ CPU사용률
+ 메모리사용현황
+ 스왑메모리 사용현황
+ 모든 프로세스들의 자원현황

2) __top명령어와 같이 사용하는 옵션(2개)__
+ `-b` : 순간의 정보를 확인(batch모드)
  + 예시: `top -b`
+ `-n` : top 실행 주기 설정(반복 횟수) 
  + 예시: `top -n 2` ( top의 실행 주기를 2번 반복한다.)

3) __top 명령어를 실행 했을 때의 화면 해석__
  ![top사진](https://user-images.githubusercontent.com/86704634/171450513-ffc8826e-33b5-4eb2-a69f-4b7e8c7b708a.png)
+ _서버정보_
  + top - 16:58:55 (시스템 현재시간)
    + 현재 서버의 시간이 16시 58분 55초
  + up 7 days, 1:15 (OS가 살아 있는 시간)
    + 7일 전에 서버가 구동 되었다. (7일째 가동중)
    + 7일 1시간 15분째 서버가 살아있다.
  + 2 USERS (유저 세션수)
    + 2명의 사용자가 접속 중  
  + load averge : 현재 시스템이 얼마나 일을 하는지 나타낸다.
    + 즉, __CPU Load의 이동평균을 표시__
    + 3개의 숫자는 1분, 5분, 15분 간의 평균 실행/대기 중인 프로세스의 수.
    + CPU 코어수보다 적으면 문제 없음 (넘어간다면, CPU에서 처리하지 못하고 대기중인 프로세스가 있다)
      + 싱글코어 경우, 1.0의 값이 CPU 100%를 사용하고 있다는 의미
      + 멀티코어 경우, 해당코어수 * N을 한 값이 CPU 100%를 사용한다는 의미
+ _프로세스 정보_
  + Task는 __현재 프로세스들의 상태__ 를 나타내주는 영역 
  + Tasks: 129 total
    + 총 129개 프로세스 가동 중
  + 1 running, 75 sleeping, 0 stopped, 0 zombie
    + 1개의 프로세스 실행 중, 75개의 프로세스가 대기 중, 0개의 프로세스 멈춤, 0개의 좀비상태
+ _CPU정보_ : 추가설명과 표 참고 / __CPU가 어떻게 사용되고 있는지 그 사용률을 보여주는 영역__
  + us란?
    + 프로세스의 우선순위 기본값보다 높은 우선순위로 사용자 공간에서 실행된 시간(ni와 반대개념)
  + ni란?
    + nice값이고 낮을수록 우선순위가 높다.
    + 프로세스 우선순위 기본값보다 낮은 우선순위로 사용자 공간에서 실행된 시간.(us와 반대개념)
  + us+ui = 사용자 공간에서 실행된 시간
  + wa란? (=wait, 시스템이 I/O(입출력) 요청을 처리하지 못한 상태에서 CPU IDLE 상태 비중)
    + I/O 입/출력을 대기하며 wait상태로 들어갈 수 있는데 이 때 즉시 실행가능한 다른 프로세스가 있으면 그 프로세스를 실행하지만, 그렇지 않은 경우에는 I/O 대기작업 중 하나가 완료될 때까지 대기해야하는데 그 시간이 wa이다.

  |용어|설명|
  |:---:|:---:|
  |us|프로세스의 유저 영역에서의 CPU 사용률|
  |sy|프로세스의 커널 영역에서의 CPU 사용률(system 레벨에서 사용중인 CPU비중)|
  |ni|프로세스의 우선순위(priority) 설정에 사용하는 CPU 사용률|
  |id|사용하고 있지 않은 비율(유후 상태의 CPU비중)|
  |wa|IO가 완료될때까지 기다리고 있는 CPU 비율|
  |hi|하드웨어 인터럽트에 사용되는 CPU 사용률|
  |si|소프트웨어 인터럽트에 사용되는 CPU 사용률|
  |st|CPU를 VM에서 사용하여 대기하는 CPU 비율|
  
+ _메모리 사용량_
  + Mem : Ram의 메모리 영역
  + Swap : 디스크를 메모리처럼 이용하는 Swap 메모리 영역, 일반적으로 Mem의 사용량이 거의 가득 찼을때 이 메모리 영역을 사용한다. (디스크이기 때문에 RAM 메모리보다 속도가 많이 느리다.)
  + total: 총 메모리 양 / free : 사용가능한 메모리 양 / used : 사용중인 메모리 양
  + buff/cache : __IO와 관련되어 사용되는 버퍼에 사용되는 메모리__
    + buff는 buffers의 약자, 이 값은 커널 버퍼에서 사용되는 메모리
    + cache는 disk의 페이지 캐시
  + avail Mem
    + 새로운 애플리케이션을 시작할 수 있는 메모리 양
    + swap 메모리를 사용하지 않고 사용할 수 있는 메모리의 크기
+ _디테일 영역_

  <img src="https://user-images.githubusercontent.com/86704634/171556081-307a7c93-0593-410f-aa56-d3521c3f252a.PNG" width="70%" height="70%"/>
  
  + PID : __프로세스 ID__, 프로세스를 구분하기 위한 겹치지 않은 고유한 값
  + USER : 프로세스를 실행한 USER 이름
  + PR & NI
    + PR: 커널에 의해서 스케줄링 되는 우선 순위
    + NI: PR에 영향을 주는 nice라는 값, 낮을수록 우선순위가 높음
  + VIRT, RES, SHR, %MEM: 프로세스 메모리와 관련
  
  |이름|설명|
  |:---:|:---:|
  |VIRT|프로세스가 소비하고 있는 총 메모리(가상메모리 전체용량, swap + res)|
  |RES|RAM에서 사용중인 메모리의 크기, 실제로 메모리를 쓰고 있는 여기가 핵심|
  |SHR|다른 프로세스와의 공유메모리를 나타냄, 예시: 라이브러리|
  |%MEM|RAM에서 RES가 차지하는 비율, 프로세스가 사용하는 메모리의 사용률|
  + %CPU : 프로세스가 사용하는 CPU의 사용률
  + S : 프로세스의 현재 상태
    + S: Sleeping(요청한 리소스 즉시 사용가능), R: Running(실행중, CPU자원소모)
    + W: sWapped out process, Z: Zombies
    + D: Uninterruptiable sleep (디스크 혹은 네트워크 I/O를 대기)
    + T: Traced or Stopped (보통의 시스템에서 자주 볼 수 없는 상태)
  + TIME+ : 프로세스가 사용한 토탈 CPU 시간
  + COMMAND : 실행된 명령어
  
4) __top 실행 후 명령어__

|단축키|설명|
|:---:|:---:|
|Shift + p|CPU 사용률 내림차순|
|Shift + m|메모리 사용률 내림차순|
|Shift + t|프로세스가 돌아가고 있는 시간 순|
|k|kill, k 입력 후 PID 번호 작성, signal은 9|
|f|soft field 선택화면 -> q 누르면 RES순으로 정렬|
|a|메모리 사용량에 따라 정렬|
|b|Batch 모드로 작동|
|1|CPU Core별로 사용량 보여줌|

+ 디테일 영역 - 원하는 값을 기준으로 정렬하는 방법
   + M: 메모리 usage에 의한 정렬
   + P: CPU usage에 의한 정렬
   + N: 프로세스 ID에 따른 정렬
   + T: running time에 의한 정렬
   + R: 오름차순과 내림차순을 토글 변경하는 단축키
+ H : 쓰레드(thread)를 기준으로 보여주는 방식
  + 요약의 Tasks영역과 디테일 영역이 변경된다.
  
  <img src="https://user-images.githubusercontent.com/86704634/171560993-94ff5c03-b428-4682-ad88-c7d55fca6b0d.jpg" width="80%" height="80%"/>
+ 필터링 기능 (o(알파벳) 또는 O(알파벳)) : COMMAND, %CPU 등등 다양한 방법으로 가능

5) __top과 ps의 차이점__
- top은 proc에서 일정 주기로 합산해 ***cpu 사용율***을 출력한다.
- ps는 ps한 시점에 proc에서 검색한 ***cpu 사용량***을 출력한다.
***
### 2) ps 명령어
1) __ps 명령어란?__
+ ***현재 실행중인 프로세스 목록과 상태***를 보여준다. (process status의 줄임말)
+ ps 명령어의 옵션은 각 시스템 계열 System V(-), BSD(-사용안함), GNU(--)마다 다른 표기법 및 출력을 가지고 있음.
+ 프로세스가 정상적인지 확인하거나 비정상적인 프로세스가 올라왔는지 확인 하는 등 리눅스 관리 전반적으로 많이 사용되는 명령어임.
+ 주로 파이프라인, grep명령어와 함께 사용하여 특정 프로세스를 확인하는데 많이 사용함.
2) __기본 구성__ (ps 명령어만 쳤을 때)
  
  ![ps명령어기본](https://user-images.githubusercontent.com/86704634/171623379-c3d31167-0523-4182-8dfb-0d9b8758b199.PNG)
+ PID: 프로세스의 ID번호(식별번호)
+ TTY: 프로세스가 수행중인 터미널(프로세스가 연결된 터미널)
+ TIME: 총 CPU 사용시간
+ CMD: 실제 프로세스의 내용(실행 명령행), 기본과 -f, -l, l옵션에서 나옴
3) __PS 항목정리__ (기본으로는 명령어 혼자치면 2번처럼 뜨지만, 옵션에 따라 더 자세한 내용을 알려주기 때문에 정리함)

|항목|의미|나오는 옵션|
|:---:|:---:|:---:|
|USER|BSD계열에서 나타나는 항목으로 프로세스 소유자의 이름|u|
|UID|SYSTEM V계열에서 나타나는 항목으로 프로세스 소유자의 이름|-f, -l, l|
|PPID|부모 프로세스 ID|
|%CPU|CPU 사용 비율의 추정치(BSD)|u|
|%MEM|메모리 사용 비율의 추정치(BSD)|u, v|
|VSZ|KB 단위 또는 페이지 단위의 가상메모리 사용량|
|SZ|프로세스가 사용하는 자료와 스택의 크기|-l, l|
|RSS|실제 메모리 사용량(Resident Set Size), KB 단위임|-l,l|
|S, STAT|현재 프로세스의 상태 코드(S:Sys V, STAT:BSD)|-l,l|
|STIME|프로세스가 시작된 시간 혹은 날짜|-f, u|
|C, CP|짧은 기간 동안의 CPU 사용률(C: Sys V, CP:BSD)|-f,l,-l|
|F|프로세스의 플래그|-l,l|
|PRI|실제 실행 우선순위, 낮을수록 우선순위가 높다|-l, l|
|NI|nice 우선순위 번호,프로세스의 우선순위 값, 낮을수록 CPU시간이 높다|-l, l|
|ADDR|프로세스 스택의 세그먼트 번호|-l, l|
|BND|커널 스레드가 바인드되는 프로세스의 논리 프로세스 번호|-o|
|COMMAND|사용자가 실행한 명령 이름|s,u,v|
|LIM|메모리에 대한 소프트 한계와 관련된 항목|v|
|SIZE|가상 이미지의 크기|v|
|TRS|텍스트의 실제 메모리 크기|v|

+ SAT : 실행되고 있는 프로세스의 상태(s, u, v옵션)
  + D : 디스크 입출력 대기 상태로 인터럽트를 걸 수 없는 상태
  + R : 실행 중일 경우
  + S : 짧은 슬립 상태
  + T : 정지 상태
  + Z : 좀비상태
  + W : 메모리에 상주한 페이지가 없는 프로세스
  + < : 높은 우선권 프로세스 / N : 낮은 우선권 프로세스
  + L : 페이지가 락이 걸린 상태

4) __주요 옵션__

|옵션|설명|
|:---:|:---:|
|-e|시스템 전체 프로세스를 보여준다.|
|-l|상세한 정보를 보여줌(긴 포멧으로 보여준다.)|
|-f|풀 포맷으로 보여준다.(UID, PID 등)|
|p, -p|특정 PID의 프로세스를 보여준다.(-e옵션과 같이 사용할 수 없음!)|
|-u|특정 사용자의 프로세스를 보여준다|

+ `ps -f` (풀 포맷으로 출력한 사진)

![ps명령어옵션f](https://user-images.githubusercontent.com/86704634/171628664-67aa82a7-1916-46d3-8e2e-5b6722ef5f98.PNG)

+ `ps -l` (긴 포맷으로 출력한 사진)

<img src="https://user-images.githubusercontent.com/86704634/171629245-fe03360a-a187-486d-84b9-c0b096dd87bd.PNG" width="70%" height="70%"/>

+ `ps -u <본인계정이름>`

<img src="https://user-images.githubusercontent.com/86704634/171643868-102aae1c-4658-41a6-a761-e43d94317460.PNG" width="40%" height="40%"/>


5) __그 외의 옵션들 정리 + 파이프라인__
+ 참고로 'a'옵션과 '-a'옵션이 있는데 이 둘은 다르다! 그 외에 엄청 많은 옵션이 있으므로 man ps를 검색하여 확인해보자. (다 정리하기엔 너무 많음)
+  파이프라인으로 주로 grep이라는 명령어와 함께 사용함. (특정 프로세스에 대해 보고싶을 경우)
+ `ps -ef | more` : 모든 프로세스를 풀 포맷으로 보여준다, more명령어를 줘서 페이지단위로 출력
  + 보통 grep으로 찾을 수 없을때 수동으로 전체 프로세스를 보고자 하는경우 사용한다.
 ![img1 daumcdn](https://user-images.githubusercontent.com/86704634/171647915-5a6e81e5-2f04-4c83-9229-9d25c4a47bca.jpg)
+ `ps -ef | grep apache` : 모든 프로세스의 출력값을 grep을 이용하여 apache가 포함된 라인들을 출력)
  + 가장 많이 사용되는 형태임.
  + 파이프라인을 이용하여 특정 패턴이 있는 프로세스를 찾아 낼 수 있다.
![img1 daumcdn](https://user-images.githubusercontent.com/86704634/171648613-9401b602-ed22-4135-90ae-a385a5f190e9.jpg)
+ `ps -el | head` : 긴 포맷으로 출력하고 싶을 경우 -l 옵션을 사용
  + 이 경우는 ps -ef에서 보이지 않았던 더 많은 정보들이 출력 된다.
  + (프로세스 상태나 우선순위를 확인하고 싶을 경우에 -l 옵션으로 확인해준다.)
    <img src="https://user-images.githubusercontent.com/86704634/171649818-1f23bedd-2433-45f8-bbfc-fef68c1550a4.PNG" width="70%" height="70%"/>
+ `ps -fp[PID]` : PID를 키워드로 프로세스 정보를 확인하는 방법이다.
---
### 3) jobs 명령어
1) __jobs 명령어란?__
+ 현재 세션의 _작업 상태_를 출력한다. (프로세스 목록을 출력해주는 명령)
  + 백그라운드로 실행중인 프로세스, 현재 중지된 프로세스, 변경 되었지만 보고되지 않은 상태 등을 표시한다.
  ![ka38_149_i2](https://user-images.githubusercontent.com/86704634/171856378-dfef3d04-8791-47aa-af46-ea5196efeefd.jpg)

+ 사용법 : `jobs [option]`

2) __옵션__

|옵션|설명|
|:---:|:---:|
|-l|프로세스 그룹 ID를 state 필드 앞에 출력한다.|
|-n|프로세스 그룹 중에 대표 프로세스 ID를 출력한다.|
|-p|각 프로세스 ID에 대해 한 행씩 출력한다.|
|command|지정한 명령어를 실행한다.|

+ `jobs -l`일 경우 사진(state 필드 앞에 프로세스 ID 출력)

![ka38_149_i3](https://user-images.githubusercontent.com/86704634/171862657-9e7d511c-c59a-417e-b208-d6bce7408fd7.jpg)

+ v로 시작하는 모든 프로세스 ID를 확인하는 방법: `jobs -p %b`

![ka38_149_i4](https://user-images.githubusercontent.com/86704634/171862898-94a67ce7-aeb1-42db-b0a9-424eaf22c248.jpg)

3) __jobs 명령어로 확인할 수 있는 세션의 상태값__

|상태|설명|
|:---:|:---:|
|Running|작업이 일시 중단되지 않았고 종료하지 않고 계속 진행 중임을 뜻한다.|
|Done|작업이 완료되어 0을 반환하고 종료했음을 뜻한다.|
|Done (code)|작업이 정상적으로 완료했으며, 0이 아닌 코드를 반환했음을 뜻한다.|
|Stopped|작업이 일시 중단됨을 뜻한다.|
|Stopped (SIGTSTP)|SIGTSTP 신호가 작업을 일시 중단했음을 뜻한다.|
|Stopped (SIGSTOP)|SIGSTOP 신호가 일시 중단했음을 뜻한다.|
|Stopped (SIGTTIN)|SIGTTIN 신호가 작업을 일시 중단했음을 뜻한다.|
|Stopped (SIGTTOU)|SIGTTOU 신호가 작업을 일시 중단했음을 뜻한다.|

***
### 4) kill 명령어
1) __kill 명령어란?__
+ 프로세스에 _특정한 signal을 보내는 명령어_
+ 일반적으로 __종료되지 않는 프로세스를 종료 시킬 때__ 많이 사용한다.

2) __사용하는 방법__
+ `kill [옵션 or 시그널(번호 또는 이름)] PID`

3) __옵션 정리__

|옵션|설명|
|:---:|:---:|
|-s|전달할 시그널의 종류를 지정한다. 여기에는 시그널 이름이나 번호를 써준다.|
|-l|시그널로 사용할 수 있는 이름들을 보여준다. 이것은 /usr/include/linux/signal.h 파일에서도 알 수 있다.|
|-1,|-HUP 프로세스를 재활성화한다.|
|-9|프로세스를 강젲로 종료시킨다.|

+ -s 명령어를 이용해 여러가지 시그널을 보낼 수 있는데, kill 명령어의 default 시그널은 TERM(15) 이다.
  + 시그널을 보내는 방법 예시
    + `kill -s KILL [pid]`
    + `kill -s SIGKILL [pid]`
    + `kill -s 9 [pid]`
    + `kill -9 [pid]` (주로 이 방법을 많이 사용한다. 프로그램 죽이기)

4) __사용 가능한 시그널의 목록__ (-l 옵션을 통해 확인할 수 있음)
+ 아래는 내 컴퓨터에 사용할 수 있는 시그널의 목록들이다. (운영체제 환경마다 시그널 목록이 다르게 보여진다.)
+ Window 환경의 시그널
<img src="https://user-images.githubusercontent.com/86704634/171966315-70620ed4-f1b1-40bf-8cff-e1c08ce58a35.PNG" width="70%" height="70%"/>

+ *핵심 몇개만 설명한 시그널 표*

|시그널|설명|시그널|설명|
|:---:|:---:|:---:|:---:|
|1.SIGHUP|연결 끊기, 프로세스의 설정파일을 다시 읽음|2.SIGINT|인터럽트|
|3.SIGQUIT|종료|4.SIGILL|잘못된 명령|
|5.SIGTRAP|트렙 추적|7.SIGBUT|버스 에러|
|8.SIGFPE|고정소수점 예외|9.SIGKILL|죽이기|
|11.SIGSEGV|세그멘테이션 위반|13.SIGPIPE|읽을 것이 없는 파이프에 대한 시그널|
|14.SIGALRM|경고 클럭|15.SIGTERM|소프트웨어 종료시그널|
|16.SIGSTKFLT|프로세서 스택 실패|17.SIGCHLD|자식 프로세서의 상태변화|
|18.SIGCONT|STOP 시그널 이후 계속 진행할 때 사용|19.SIGSTOP|정지|
|20.SIGTSTP|키보드에 의해 발생하는 시그널|

+ 블로그 글 (OSX에서 실행한 화면) - 출처는 맨아래 참고자료 사이트에 적어져 있음.
<img width="410" alt="99E84B455C6378A109" src="https://user-images.githubusercontent.com/86704634/171966499-dcada309-56af-483e-9303-e5dabbe07b35.png">

***
## vim 에디터
### 1) 매크로 사용방법(q, @) : 같은 동작을 반복하는 기능이다.

+ __기본적인 매크로 사용방법__
  + 커맨드 모드 (esc 누른 상태)에서,
  1) 'q'를 누르고 a~z 사이 문자로 매크로 recording 시작
  2) 커맨드 모드로 돌아와서 'q'를 누르면 매크로 recording 을 끝낸다.
  3) 매크로를 사용하려면 커맨드 모드에서 @ + a~z 를 입력하면 끝

+ 같은 명령 반복하는 매크로 기능 (위와 같은 설명 + 4번에 단축키 적혀있음)
  1) q + a : a키에 recording 시작
  2) 반복을 위한 내가 원하는 동작 후 명령모드로 돌아가기
  3) q : recoding 종료
  4) @a : 1회 실행 / @@ : 방금 실행한 매크로 실행 / 10@a : 매크로 10회 실행

+ __자주 사용하는 매크로 등록 방법__
  1) ~/.vimrc를 연다.
  2) let @a = '매크로 문자열' // 이런식으로 매크로로 동작시킬 문자열을 입력한다.
  + ex) 매크로 이름을 b라고 짓고 싶다
    + let @b = ' 까지 치기
    + insert모드에서 Ctrl-R -> Ctrl-R -> b 순으로 누르면 매크로의 b의 내용물이 입력 될 것임.
    + 내용물이 입력 되었으면 '를 마저 닫아준다. 

+ _매크로 문자열을 편집기에 그대로 출력하는 방법_ (참고내용으로 적어두었음)
  1) 첫 번째 방법 : 편집모드에서 ctrl + r, ctrl + r, 매크로 문자 를 순서대로 입력하면 그대로 출력된다.
  + 단점 : 방향키 등의 특수키 문자가 제대로 붙여넣기 불가능
  + 분명히 출력 결과물이 같아 보이지만, 실제 바이너리를 뜯어보면 다르게 출력됨을 알 수 있음
  2) 두 번째 방법 : 커맨드 모드에서 "매크로문자p 를 입력하여 래지스터로부터 바로 붙여넣기 한다. ( 예시: "ap, "bp, "cp, "dp, ....)
***
### 참고자료(사이트)
1) top 명령어 자료
- <https://cheershennah.tistory.com/172>
- <https://zzsza.github.io/development/2018/07/18/linux-top/>
- <https://blog.naver.com/dktmrorl/222635202537>
- [네이버 지식백과](https://terms.naver.com/entry.naver?docId=4125861&cid=59321&categoryId=59321 "top용어사전")
2) ps 명령어 자료
- [리눅스 ps명령어 사전, 네이버 지식백과](https://terms.naver.com/entry.naver?docId=4125773&cid=59321&categoryId=59321)
- <https://arer.tistory.com/150>
- <https://newstars.cloud/468>
- [네이버블로그-요약](https://blog.naver.com/dktmrorl/222416977486 "ps명령어 요약")
- <https://reakwon.tistory.com/183>
- <https://jhnyang.tistory.com/268>
3) jobs 명령어 자료
- [리눅스 명령어사전 jobs - 네이버 지식백과](https://terms.naver.com/entry.naver?docId=4125682&cid=59321&categoryId=59321)
- <https://starrykss.tistory.com/1694>
- <https://hbase.tistory.com/265>
4) kill 명령어 자료
- [리눅스 명령어 사전 kill - 네이버 지식백과](https://terms.naver.com/entry.naver?docId=4125687&cid=59321&categoryId=59321)
- https://bigsun84.tistory.com/355
- https://sisiblog.tistory.com/209
5) 매크로 사용방법 자료
- https://stdout.tistory.com/46
- https://forcecore.tistory.com/1255
- https://booolean.tistory.com/849
