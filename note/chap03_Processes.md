# chapter 3 : Processes

2021.03.19



### Multiprogramming

* 운영체제의 기본적인 동작원리 중에 하나로 프로그램을 실행시키는 방식

* Uni-programming:  한번에 하나 씩 실행시키는 방식

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image01.PNG?raw=true" alt="image01" style="zoom:67%;" />

  1. 메모리에 적재되어있는 프로그램이 실행됨
  2. CPU에 의해 프로그램 A가 먼저 실행됨
  3. I/O 장치를 통해 프로그램 A가 입출력을 하고 CPU는 idle 상태가 됨
  4. 입출력이 끝나며 A가 다시 CPU를 사용하고 A가 종료됨
  5. 프로그램 B와 C도 같은 방식으로 실행됨

  -> CPU가 사용되는 시간이 적으며 CPU 자원이 낭비됨

* Multi-programming

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image02.PNG?raw=true" alt="image02" style="zoom:67%;" />

  1. 메모리에 적재되어있는 프로그램이 실행됨
  2. 프로그램 A가 CPU를 통해 실행됨
  3. 프로그램 A가 입출력을 할 때 CPU를 프로그램 B가 사용할 수 있게 전환됨
  4. 프로그램 B가 입출력을 할 때 CPU를 프로그램 C가 사용할 수 있게 전환됨
  5. 실행할 프로그램이 있으면 이와 같은 과정이 반복됨

  -> CPU가 계속해서 사용되며 CPU 자원을 효율적으로 사용할 수 있음 (생산성이 좋아짐)

* Multiprogramming은 생산성(Throughput)이 향상되고 CPU의 활용도(Utilization)가 크게 좋아지며 동시에 사용되는 프로그램 갯수(Degree of multiprogramming)가 많아짐



### 3.1 Process Concept

---

* 운영체제마다 실행중인 프로그램을 부르는 용어

  * Batch system(일괄처리 시스템) - job

  * Time shared systems  – user programs or tasks

  * 앞으로 실행중인 프로그램을 job 또는 process 라고 부르겠습니다.

  * Process: 실행중인 프로그램

  * 메모리 상의 Process 이미지

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image03.PNG?raw=true" alt="image03" style="zoom:60%;" />

    * 프로그램이 실행을 위해서 메모리에 로드된 상태로 메모리 상의 주소는 0과 max 같은 주소로 표현함
    * stack과 heap은 프로그램 실행에 따라 크기가 변할 수 있음

    * 프로세스의 메모리 구성
      * text: 프로그램의 명령인 코드 부분에 해당함 (각종 CPU 레지스터도 포함)
      * stack: 프로그램이 시작한 뒤 함수의 실행을 담당함
      * data: 전역변수들이 위치한 영역
      * heap: 프로그램 실행중에 동적으로 할당할 때 사용하는 영역

* Process Concept (프로세스 개념)

  * Program: CPU 명령들을 내용으로 가진 디스크에 저장되어 있는 수동적인 존재
  * Process: 실행파일이 이에 해당함 CPU 명령을 지정하는 프로그램 카운터 레지스터와 실행에 필요한 자원 집합을 가진 능동적인 존재
  * 마우스 클릭이나 명령줄(command line)을 통해 프로그램 실행
  * 프로그램은 하나지만 연관된 process는 여러 개가 될 수 있음

* Process State (프로세스 상태)

  *  new: process가 만들어 지고 있는 상태
  * ready: process가 CPU에 할당되기를 기다리는 상태 
  * running: process가 실행중인 상태 (running 상태의 process는 1개 밖에 없음)
  * waiting: process가 입출력 완료를 기다리고 있는 상태 
  * terminated: process가 실행을 마친 상태

* Diagram of Process State (프로세스의 상태 전이)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image04.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * admitted: new 프로세스 중에 정식으로 프로세스가 될 대상을 선택해서 해당 프로세스를 ready로 만듦
  * scheduler dispatch: ready 프로세스 중에 선택하고 CPU를 주어 실행을 시작시킴
  * interrupt: time-sharing system의 실행중인 프로세스가 오랫동안 실행중이어서 운영체제가 프로세스로부터 CPU를 빼앗는 경우
  * I/O  or event wait: 실행중인 프로세스가 입출력을 요청하는 경우
  * I/O or event completion: 입출력을 요청하고 입출력의 완료를 기다리는 프로세스에게 입출력의 완료 interrupt가 발생하는 경우
  * exit: 실행중인 프로세스가 종료하는 시스템콜을 호출하게 되어 운영체제가 해당 프로세스의 상태를 바꾸는 경우

* Process Control Block (PCB)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image05.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * 커널이 개별 프로세스를 유지하기 위해서 관리하는 자료구조
  * 프로세스 한개당 하나의 PCB가 있으며 한 프로세스에 해당하는 모든 정보가 담겨있음
  * PCB에 담긴 정보
    * Process state (프로세스의 상태) - running, waiting 등
    * Process ID - 보통 정수로 표현
    * Program counter – 현재 실행중인 명령의 위치를 담고 있음
    * CPU registers – Program counter를 제외한 모든 CPU 레지스터의 값들을 담고 있음
    * CPU scheduling information - 커널이 CPU scheduling을 위해 필요한 정보들이 담겨 있음 (우선순위, 포인터 등)
    * Memory-management information – 프로세스가 사용하는 메모리의 위치 정보가 담겨있음
    * Accounting information – CPU의 자원 사용 기록이 담겨있음
    * I/O status information – 프로세스에 할당된 입출력장치, 오픈파일 등 프로세스의 입출력에 관련된 정보들이 담겨있음

* CPU Switch From Process to Process (P0에서 P1으로 CPU 스위치)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image06.PNG?raw=true" alt="image03" style="zoom:60%;" />

  1. 프로세스 P0가 실행됨
  2. interrupt or system call에 의해 P0가 실행을 중단하고 P0의 상태를 PCB0에 저장함
  3. P1의 PCB1에서 P1의 상태를 복원하여 CPU가 P1으로 전환하여 프로세스 P1이 실행됨
  4. interrupt or system call에 의해 P1이 실행을 중단하고 P1의 상태를 PCB0에 저장함
  5. P0의 PCB0에서 P0의 상태를 복원하여 CPU가 P0로 전환하여 프로세스 P0가 실행됨

* Process Representation in Linux

  `Linux에서 PCB 자료구조는 C언어의 구조체 형식으로 구성되어 있음`

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image07.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * Doubled Linked Process로 관리함

### 3.2 Process Scheduling

---

* Process Scheduling

  * time sharing: 프로그램이 실행되는 동안에 사용자와 상호작용할 수 있도록 프로세스간의 CPU 교체

  * Process Scheduler: 실행가능한 여러개의 프로세스 중 하나의 프로세스를 선택하는 작업을 함

  * scheduling queues

    `scheduler의 선택 대상이 되는 프로세스들을 모아놓은 queue`

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image08.PNG?raw=true" alt="image03" style="zoom:60%;" />

    * Ready queue: 메인메모리에 있으면서 즉시 실행가능한 프로세스의 queue
    * Device queues: 장치마다 하나씩 존재하고 해당 디바이스에 대해 입출력을 개시하고 완료를 기다리는 프로세스가 속하는 queue
    * Job queue: 시스템에 있는 모든 process가 포함되어 있는 queue
    * 프로세스는 자신의 life cycle 동안에 여러 scheduling queue를 옮겨 다니게 됨



* Schedulers

  * Short-term scheduCPU scheduler)
    * 다음 번에 실행할 프로세스를 선택하여 해당 프로세스에 CPU를 할당하는 역할
    * 매우 빈번하게 실행됨 (milliseconds)  ->(must be fast)
  * Long-term scheduler (or job scheduler)
    * 어느 프로세스를 ready queue로 갖고 올지를 결정
    * short-term 과 같이 빈번하게 동작하지 않음 (seconds, minutes) -> (may be slow)
    * degree of multiprogramming을 조절
  * Process의 종류
    * I/O-bound process : CPU를 사용하는 계산보다 입출력 위주로 동작하는 프로세스, CPU bursts 가 짧음
      * CPU bursts: 프로게스가 CPU를 사용하는 구간 
    * CPU-bound process : 입출력보다 계산을 위주로 하는 프로세스, CPU bursts 가 긺

  `Long-term scheduler는 I/O bound와 CPU bound가 적절하게 섞일 수 있도록 구성함`

* Addition of Medium Term Scheduling

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image09.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * Medium-term scheduler: 일부 프로세스를 swap 디바이스인 디스크영역으로 내쫓는 역할
    * swapping: 시스템의 프로세스 수가 많은 경우 메모리의 빈공간을 확보하려고 디스크 공간으로 보냈다가 다시 불러오는 과정

* Context Switch (문맥 교환)

  * CPU가 교환될 때 이전 프로세스의 상태를 저장하고 다음 프로세스의 상태를 복원하는 과정
  * Context: 사용중인 CPU 레지스터 값, 프로세스의 메인메모리 상의 내용을 통칭하는 용어
  * Context-switch time은 overhead 이므로 자주하지 않고 짧게 하는것이 좋음



### 3.3 Operations on Processe

---

* 대표적인 프로세스 연산
  * process creation, process termination, and so on as detailed next
  * 대부분 system call로 주어짐
* Process Creation
  * 프로세스는 커널이 제공하는 프로세스 생성연산을 통해 새로운 프로세스를 만들 수 있음
    * Parent, Child
    * 프로세스로 이루어진 tree가 생성됨
  * init process: 부모 프로세스가 없는 유일한 프로세스로 커널이 직접 만듦
  * pid: 모든 프로세스가 갖고 있으며 이를 통해 프로세스르 식별
  *  Resource sharing options (자원의 공유 측면)
    * Parent, Child 프로세스 간 자원을 공유하는 모델, 일부만 공유하는 모델, 전혀 공유하지 않는 모델로 나누어짐
  *  Execution options (실행 측면)
    * Parent, Child 프로세스가 동시에 실행할 수 있음
    * Parent 프로세스가 Child 프로세스 수행의 종료를 기다릴 수 있음



* Process Creation (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image10.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * Address space
    * Parent 프로세스의 주소공간을 그대로 복제하여 Child 프로세스의 주소공간으로 지정
    * Child 프로세스는 Parent 프로세스와 내용이 동일한 메모리 상의 이미지를 갖게됨
  * UNIX examples
    * 새로운 프로세스 생성에 fork()라는 시스템콜이 사용됨
    * Child 프로세스는 생성 직후에 exec() 시스템콜을 실행하여 시스템콜에서 지정한 실행파일의 내용으로 교체

* C Program Forking Separate Process

  `새로운 프로세스를 생성하고 이를 통해 새 프로그램을 실행시키는 프로그램`

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image11.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * 새 프로세스 id를 저장할 변수 pid 정의
  * fork() 시스템콜로 자식 프로세스 생성
    * 성공적으로 return하는 경우: 자식, 부모 프로세스에 모두 반환함
    * fork() 시스템콜의 반환값
      * Parent 프로세스: 새로 만들어진 Child 프로세스의 pid (보통 양수값)
      * Child 프로세스: 0이 주어짐
  * if 문 설명
    * pid < 0 : fork가 실패하여 새로운 프로세스 생성이 이루어지지 않음
    * pid == 0 : Child 프로세스가 실행하게 됨
      * execlp 시스템콜이 실행됨
    * pid > 0 : Parent 프로세스가 실행됨
      *  wait 시스템콜에 의해 자식 프로세스의 종료를 기다림



* Creating a Separate Process via Windows API

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image12.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * 새 프로세스의 특성을 갖는 변수인 si와 식별자 pi 정의
  * 변수 si와 pi 0으로 초기화
  * 새로운 프로세스를 생성하여 프로그램을 실행시키는 CreateProcess 함수를 호출
  * WaitForSingleObject 시스템콜로 부모 프로세스가 자식 프로세스의 종료를 기다림

* Process Termination

  * exit() 
    * 시스템콜로 정상종료하는 시스템의 종료기능을 수행함
    * 1개의 정수 파라미터를 갖고 이 값을 부모 프로세스에게 전달
  * abort()
    * 자식 프로세스를 강제 종료시키기 위해서 부모 프로세스가 abort() 시스템콜을 사용하여 자식 프로세스를 종료시킴
    * 자식 프로세스의 자원 사용량이 지나치게 많거나 수행하는 작업이 필요없거나 부모 프로세스가 종료될 때 사용함
  * 부모 프로세스가 자식 프로세스보다 먼저 종료하는 경우
    * cascading termination: 부모 프로세스가 종료되어 자식 프로세스와 하위 프로세스를 강제 종료시키는 시스템콜
    * 부모 프로세스는 wait() 시스템콜로 자식 프로세스의 종료를 기다림
      * 자식 프로세스의 pid를 반환함
      * zombie: 자식 프로세스가 exit 했지만 wait 시스템콜을 하지 않은 경우 
      * orphan: 부모 프로세스가 종료했지만 자식 프로세스는 실행중인 경우



### 3.4 Interprocess Communication

---

* Interprocess Communication

  * 프로세스는 independent(독립적)이며 cooperating(협력적)이다.
  * cooperating - 정보를 공유하고 작업을 나누어 실행하며 여러 프로세스를 실행할 경우
  * 프로세스간 통신인 IPC 기법이 필요함 
  * IPC 기법: Shared memory, Message passing

* Interprocess Communication – Shared Memory

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image13.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * 통신을 하려고 하는 두 프로세스가 일정한 메모리영역을 공유함으로서 데이터를 주고받는 방법
  * 통신, 수신 동작이 사용자 프로세스의 통제하에 이루어짐
  * 두 프로세스의 동작이 동기화되어야 함

* Producer-Consumer Problem (프로세스 동기화 문제 예시)

  * 두 개의 프로세스인 producer와 consumer가 있고 buffer라는 공간을 공유하면서 producer는 buffer에 데이터를 생산하여 채워넣고 consumer는 buffer에서 데이터를 꺼내어 소비하는 역할
    * unbounded-buffer places no practical limit on the size of the buffer
    * bounded-buffer assumes that there is a fixed buffer size



* Bounded-Buffer – Shared-Memory Solution

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image14.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * bounded buffer의 크기를 BUFFER_SIZE인 10으로 지정
  * buffer에 저장된 데이터 타입을 구조체 형태로 정의
  * 실제 buffer를 배열을 사용하여 정의
  * 정수형 변수 in, out은 buffer의 위치를 알려주는 포인터 역할

* Bounded-Buffer – Producer and Consumer

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image15.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * Producer
    * 데이터를 생산
    * 데이터를 버퍼에 저장
      * bounded buffer의 상태를 확인하고 데이터를 저장한 다음 in 포인터 변수 수정
  * Consumer
    * 데이터를 소비
    * 데이터를 수신하는 과정
      * bounded buffer의 상태를 확인하고 데이터를 수신한 다음 out 포인터 변수 수정



* Interprocess Communication – Message Passing

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image16.PNG?raw=true" alt="image03" style="zoom:60%;" />

  * 프로세스가 통신을 하거나 동기화를 하기 위한 IPC 매커니즘
  * 프로세스는 메모리를 공유하지 않고도 통신을 할 수 있음
  * 메시지 passing 방식
    * send(message)
    * receive(message)
  * 주고 받는 메시지의 크기 - 불변, 가변

* naming: 데이터를 주고받을 때 프로세스를 가르키기 위한 방법으로 이를 통해 Direct 와 Indirect Communication direct으로 나뉨

* Direct Communication

  * 송신을 원하는 각 프로세스가 송신의 송신자, 수신자 이름을 명시해야함
    * send (P, message) – send a message to process P
    * receive(Q, message) – receive a message from process Q
  * 특징
    * 연결이 자동적으로 이루어짐
    * 연결을 오직 두 프로세스에서만 이루어짐 (단방향, 양방향)

* Indirect Communication

  * 프로세스 간에 메일박스로 (port) 메시지를 주고 받음
    * 메일박스마다 특정한 id가 있으며 통신하기 위해서 특정한 id의 메일박스를 프로세스 간에 공유해야함
    * 특징
      * 공통의 메일박스를 갖는 프로세스 간에만 링크가 연결됨
      * 한 링크를 통해서 여러 통신이 가능함
      * 한 쌍의 프로세스간 여러 개의 링크를 공유할 수 있음 (단방향, 양방향)  
  * Operations
    * 메일박스를 생성하고 제거하는 연산
    * 메일박스를 통해 메시지를 송신하거나 수신하는 연산
  * Primitives
    * send(A, message) – send a message to mailbox A 
    * receive(A, message) – receive a message from mailbox A
  * Mailbox sharing
    * P1, P2, and P3 share mailbox A
    * P1, sends; P2 and P3 receive
    * 누가 실제로 메시지를 수신할 것인가?
      * Allow a link to be associated with at most two processes
      * Allow only one process at a time to execute a receive operation
      * Allow the system to select arbitrarily the receiver. Sender is notified who the receiver was.

* Synchronization

  * Blocking (synchronous)

    * Blocking send - 보낸 메시지를 receiver가 수신을 할 때까지 send가 block 되는 과정
    * Blocking receive - 수신을 하고자 하는 메시지가 가용해질때까지 receiver가 block 되는 과정

  * Non-blocking (asynchronous)

    * Non-blocking send - 메시지를 보낸 후 수신 여부에 관계없이 다음 동작을 이어나가는 방식
    * Non-blocking receive - 수신할 수 있는 메시지가 없는 경우 Null 메시지를 받음

  * Producer-consumer becomes trivial

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image17.PNG?raw=true" alt="image03" style="zoom:60%;" />

    * 프로세스가 주고받을 데이터를 메시지로 선언
    * 생산자는 데이터를 생산하고 메시지로 송신
    * 소비자는 수신받을 데이터를 메시지로 선언하고 생산자가 보낸 데이터를 저장한 뒤 소비



* Buffering

  * 송신 중인 메시지를 임시로 저장하는 Queue
  * 메시지 Queue를 구현하는 방법
    1. Zero capacity: queue의 최대길이가 0이며 link에는 대기하는 메시지가 없고 receiver가 수신을 할 때까지 기다려야 함
    2. Bounded capacity: buffering 하는 공간의 크기가 제한되어 있는 경우로 가득차있는 경우 queue가 비워지기를 기다려야 함 
    3. Unbounded capacity – queue의 길이에 제약이 없는 경우로 sender가 기다리는 경우가 없음

* Multprogramming 에서의 프로세스간 Contact Switch

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap03/image18.PNG?raw=true" alt="image03" style="zoom:60%;" />

