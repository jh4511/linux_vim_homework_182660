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
+ CMD: 실제 프로세스의 내용(실행 명령행)
3) __PS 항목정리__ (기본으로는 명령어 혼자치면 2번처럼 뜨지만, 옵션에 따라 더 자세한 내용을 알려주기 때문에 정리함)

|항목|의미|
|:---:|:---:|
|USER|BSD계열에서 나타나는 항목으로 프로세스 소유자의 이름|
|UID|SYSTEM V계열에서 나타나는 항목으로 프로세스 소유자의 이름|
|PPID|부모 프로세스 ID|
|%CPU|CPU 사용 비율의 추정치(BSD)|
|%MEM|메모리 사용 비율의 추정치(BSD)|
|VSZ|K단위 또는 페이지 단위의 가상메모리 사용량|
|RSS|실제 메모리 사용량(Resident Set Size)|
|S, STAT|현재 프로세스의 상태 코드(S:Sys V, STAT:BSD)|
|STIME|프로세스가 시작된 시간 혹은 날짜|
|C, CP|짧은 기간 동안의 CPU 사용률(C: Sys V, CP:BSD)|
|F|프로세스의 플래그|
|PRI|실제 실행 우선순위|
|NI|nice 우선순위 번호|
4) __주요 옵션__

|옵션|설명|
|:---:|:---:|
|-e|시스템 전체 프로세스를 보여준다.|
|-l|상세한 정보를 보여줌(긴 포멧으로 보여준다.)|
|-f|풀 포맷으로 보여준다.(UID, PID 등)|
|p, -p|특정 PID의 프로세스를 보여준다.|
|-u|특정 사용자의 프로세스를 보여준다|

+ `ps -f` (풀 포맷으로 출력한 사진)

![ps명령어옵션f](https://user-images.githubusercontent.com/86704634/171628664-67aa82a7-1916-46d3-8e2e-5b6722ef5f98.PNG)

+ `ps -l` (긴 포맷으로 출력한 사진)

<img src="https://user-images.githubusercontent.com/86704634/171629245-fe03360a-a187-486d-84b9-c0b096dd87bd.PNG" width="70%" height="70%"/>

5) __그 외의 옵션들 정리__
6) 
 
---
### 3) jobs 명령어
***
### 4) kill 명령어

## vim 에디터
### 1) 매크로 사용방법(q, @)

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
4) kill 명령어 자료
5) 매크로 사용방법 자료
