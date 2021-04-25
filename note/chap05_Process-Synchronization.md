# chapter 5 : Process Synchronization

2021.04.06

`process 간 실행의 동기화`

```markdown
* 주요내용
- 동기화가 어떤 문제이며 문제가 생긴 배경 설명
- Critical-Section Problem의 개념과 상황
	1. Peterson's Solution
	2. 하드웨어적인 Solution
	3. 보편적으로 많이 사용되는 동기화 도구인 Mutex Locks와 Semaphores
- 고전적인 동기화 문제
- 프로그래밍 언어가 제공하는 동기화 도구인 Monitor에 대해 알아봄
```



## 5.1 Background

* Background
  * 프로세스는 모두 concurrent하게 수행(병행 수행)
  * 개별 프로세스는 자신의 코드를 수행 중에 실행이 중단되어 다른 프로세스가 실행될 수 있음
  * 다수의 프로세스간 데이터를 공유하여 공유데이터 접근 중 실행이 중단되는 경우에 데이터의 consistency가 깨질 수 있음
  * Process Synchronization: 병행 실행하는 프로세스가 공유 데이터를 접근할 경우 데이터의 consistency가 유지되도록 하는 방법
  * cooperating process: 동기화되어 실행되는 프로세스



* Producer-consumer problem (3장에서 공부한 문제)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image01.PNG?raw=true" alt="image" style="zoom:67%;" />

  * counter: 현재 버퍼내에 저장되어 있는 데이터 개수
  * 생산자, 소비자 버퍼는 독립적인 프로세스로 공유 변수인 counter를 같이 사용함

* Race Condition 예시

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image02.PNG?raw=true" alt="image" style="zoom:67%;" />
  
  * Race Condition: 병행 프로세스들이 공유 데이터를 사용하다가 실행이 중단되어 데이터의 consistency가 깨지는 경우



## 5.2 Critical-Section Problem

* Critical-Section Problem
  * n개 프로세스가 있을 때를 가정 - P0, P1, P2, P3, .., Pn
  * critical section: 프로세스의 코드 중에서 공유 데이터를 접근하는 부분 (임계 구역)
    * 프로세스가 공유 변수를 바꾸는 경우, updating table, writing file
    * 한 프로세스가 critical section을 실행하는 동안에 나머지 프로세스는 critical section을 실행하면 안됨
  * Critical section problem: 한 번에 한 프로세스가 critical section을 수행할 수 있게 protocol을 설계하는 문제
  * critical section과 공유 데이터 접근과 관련없는 remainder section으로 코드를 나누어 생각함
  * entry section, exit section: critical section의 진입 직전 동작, 진입 후 동작
  * 적절한 entry section과 exit section을 정하여 critical section problem을 해결



* Solution of Critical Section Problem

  * Critical Section Problem 예시
  
  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image03.PNG?raw=true" alt="image" style="zoom:67%;" />
  
  * Critical Section Problem이 충족해야하는 요구조건 (3가지)
    1. Mutual Exclusion: Pi가 자신의 critical section을 실행하는 동안 다른 프로세스는 critical section을 실행할 수 없음
    2. Progress: critical section을 실행하는 프로세스가 없고 일부 프로세스들이 자신의 critical section에 진입할 프로세스의 결정에 참여할 수 있으며 이 결정은 무한정 연기될 수 없음
    3. Bounded Waitting: 한 프로세스가 자신의 critical section에 진입하려고 시도할 때 다른 프로세스들이 critical section에 진입하도록 허용하는 횟수에 한계가 있어야함
  * 각 프로세스의 실행 속도에 있어서 일정한 가정을 하지 않음



## 5.3 Peterson’s Solution

* Peterson’s Solution

  * Critical Section Problem을 해결하는 간단한 해결책

  * 두 프로세스의 해결책이며 문제를 소프트웨어간 해결

  * CPU 명령 중에 `load` 와 `store` 이 atomic 함 (순차적으로 진행)

    *  `load`: 메모리의 특정 위치에 있는 데이터 값을 CPU 내의 레지스터로 복사하는 동작
    *  `store`: CPU 레지스터의 값을 메모리의 특정 위치에 복사하는 동작

  * 2개의 프로세스 2개의 변수를 공유함

    ```java
    int turn;
    Boolean flag[2];
    ```

  * 변수 `turn`: 어느 프로세스가 자신의 critical section에 진입할 차례인지를 나타내는 변수

* Algorithm for Process P0 and P1

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image04.PNG?raw=true" alt="image" style="zoom:67%;" />

  * entry section
    * flag[0]를 true로 바꾸고 turn을 1로 지정
    * while문 안의 조건이 만족하지 않아야 critical section에 진입함 (flag[1]이 false 이거나 turn이 0인 경우)
  * exit section



* Peterson’s Solution (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image05.PNG?raw=true" alt="image" style="zoom:67%;" />
  
  * Critical Section Problem이 충족해야하는 요구조건이 (3가지) 모두 만족하는 확인
    1. Mutual exclusion - 두 프로세스가 critical section에 동시에 있을 수 없으므로 만족
    2. Progress - 프로세스들이 자신의 critical section에 진입할 결정에 참여할 수 있으며 무한정 연기될 수 없으므로 만족



## 5.4 Synchronization Hardware

* Synchronization Hardware

  * 대부분의 컴퓨터 시스템에서는 하드웨어 지원을 제공함
  * 단일 CPU 시스템 (Uniporcessor): interrupt를 끄는 방법으로 간단하게 해결 가능
    * 현재 실행중인 코드가 방해를 안받고 실행을 진행할 수 있음
    * multiprocessor 시스템에서 사용할 수 없고 시스템의 효율이 떨어짐
  * 현대의 컴퓨터는 CPU 명령 중에 메모리 워드를 테스트하고 변경하거나 두 메모리 내용을 바꾸는 명령을 제공함
    * Atomic: 명령의 실행이 interrupt 되지 않음

* test_and_set Instruction

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image06.PNG?raw=true" alt="image" style="zoom:67%;" />

  * target 메모리 변수를 rv에 저장하고 target의 값을 TRUE로 지정한 다음 rv를 반환함
  * 동작이 atomical하게 이루어짐

* Solution using test_and_set

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image07.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 공유 변수 lock의 초기값은 false
  * test_and_set에 의해 critical section에 진입하고 lock의 값은 true로 바뀜
  * lock을 false로 바꾸고 반복문을 빠져나옴



* compare_and_swap Instruction

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image08.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 특정한 메모리의 값을 읽어들임
  * 현재의 value의 값을 temp에 저장하고 현재 값이 expected와 같은 경우 값을 바꾸고 초기값인 temp를 반환함

* Solution using compare_and_swap

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image09.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 공유 변수 lock의 초기값은 0

* Bounded-waiting Mutual Exclusion with test _and_set

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image10.PNG?raw=true" alt="image" style="zoom:67%;" />
  
  * bounded waiting 까지 만족하는 solution
  *  공유 변수: `boolean lock`, `boolean waiting[n]` 



## 5.5 Mutex Locks

* Mutex Locks (spinlock)

  * 앞에서 배운 해결책은 복잡하며 응용 프로그래머들이 직접 사용할 수 없음
  * OS 설계자들이 critical section problem을 해결하기 위해 개발한 도구
  * 임계지역 진입 가능 여부를 나타내는 Boolean 변수로 `acquire()` 함수로 호출하고 lock을 얻어서 임계지역에 진입하거나 `release()` 함수를 통해서 lock을 해제할 수 있음
  * `acquire()`와 `release()` 함수는 atomic하게 작동함

* acquire() and release()

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image11.PNG?raw=true" alt="image" style="zoom:67%;" />

  * `acquire()`  - while문 형태로 lock의 현재 값을 검사하는 형태
  * `release()` 



## 5.6 Semaphores

* Semaphores

  * Mutex lock에 비해 정밀한 제어를 할 수 있는 동기화 도구

  * S로 표기하며 정수형 변수

  * 2개의 atomic한 연산으로 접근

    * wait() 또는 P()
    * signal() 또는 V()

  * 동작하는 방식

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image12.PNG?raw=true" alt="image" style="zoom:67%;" />

    

* Semaphore Usage

  * Counting semaphore: 변수가 0부터 n 사이의 값을 갖고 시스템 내의 가용자원의 개수를 나타내는데 사용
  * Binary semaphore: 변수가 0 또는 1의 값을 갖고 mutex lock과 같은 효과

* Semaphore Implementation

  * 같은 semaphore에 대해서 두 프로세스가 동시에 wait나 signal을 할 수 없음
  * wait와 signal의 코드가 critical section 안에 있음
  * busy waiting을 통해 간단히 구현할 수 있음



* Semaphore Implementation with no Busy waiting

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image13.PNG?raw=true" alt="image" style="zoom:67%;" />

  `Busy waiting을 피할 수 있는 semaphore의 구현 방법`

  * semaphore마다 waiting queue를 만듦'
  * 2개의 연산
    * block: 프로세스를 wait queue에 넣는 동작
    * wakeup: wait queue에 대기중인 프로세스가 있는 경우 이를 꺼내서 ready queue로 옮기는 동작



* Implementation with no Busy waiting (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image14.PNG?raw=true" alt="image" style="zoom:67%;" />

* Deadlock and Starvation

  `semaphore 잘못 사용하면 생길 수 있는 문제`

  * Deadlock: 둘 이상의 프로세스가 영원히 발생하지 않을 사건을 무한정 기다리는 상황

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image15.PNG?raw=true" alt="image" style="zoom:67%;" />

  * Starvation: 프로세스가 wait queue에서 영원히 빠져나오지 못하는 경우

  * Priority Inversion: 우선순위 역전 문제

    

## 5.7 Classic Problems of Synchronization

* Classic Problems of Synchronization (동기화 문제)
  * Bounded-Buffer Problem, Readers-Writers Problem, Dining-Philosophers Problem



* Bounded-Buffer Problem

  * 버퍼의 크기는 n이고 semaphore인 mustex(초기값 1), full(초기값 0), empty(초기값 n) 를 사용
  * Bounded-Buffer Problem (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image16.PNG?raw=true" alt="image" style="zoom:67%;" />

  * wait(empty): 생산자는 버퍼에 넣기위해서 빈 버퍼 공간을 확보 - signal(empty)
  * 버퍼를 접근하는 부분: mutex를 사용하여 보호
  * signal(full): 생산자가 버퍼를 추가하여 버퍼의 개수 증가 - signal(empty)



* Readers-Writers Problem

  * 다수의 병행 프로세스가 공유 데이터 집합을 접근하는 상황 - Readers, Writers
  * reader가 여러 개일때 동시에 데이터를 접근할 수 있지만 writer는 한 번에 한 writer 프로세스만 공유 데이터에 접근할 수 있음
  * 우선순위를 부여하는 방식으로 문제 해결
  * 공유 데이터: Data set, read_count(초기 0), rw_mutex(초기 1), mutex(초기 1)

* Readers-Writers Problem (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image17.PNG?raw=true" alt="image" style="zoom:67%;" />

  * Writer Process
    * wait(rw_mutex) - 공유 데이터를 접근할 때 상호배제가 이루어져야함
  * Reader Process
    * 여러 reader가 동시에 공유 데이터 접근이 가능함
    * read_count: 공유 데이터를 동시에 접근하는 reader의 수



* Dining-Philosophers Problem

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image18.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 5명의 철학자들이 원탁에 둘러앉아 있고 돌아가면서 음식을 먹음
  * 공유 데이터: Bowl of rice, chopstick[5] (초기 1)

* Dining-Philosophers Problem Algorithm

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image19.PNG?raw=true" alt="image" style="zoom:67%;" />

* Dining-Philosophers Problem Algorithm (Cont.)

  * Deadlock handling
    * 좌석은 5개지만 4명의 철학자가 앉음
    * 젓가락 2개를 모두 집을 수 있을 때만 젓가락을 들도록 규칙을 정함
    * 비대칭적인 해결책: 홀수와 짝수를 나눠서 젓가락을 집는 순서를 정함



## 5.8 Monitors

* Monitors

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image20.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 프로세스간 동기화를 편하게 할 수 있는 고급언어 construct로 abstract data type
  * 모니터 타입의 예
    * 동기화와 관련된 공유변수, 변수에 대한 함수, 초기화 코드
  * 모니터 안에서 항상 하나의 프로세스만이 활성화되도록 보장해줌
  * 모니터 사용자가 부가적인 동기화 기법을 직접 정의할 때 Condition Variable이 사용됨



* Condition Variables

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image27.PNG?raw=true" alt="image" style="zoom:67%;" />
  
  * 기본적인 모니터가 지원하지 못하는 정교한 정규화를 위해서 사용됨
  * condition x, y
  * x.wait(): 프로세스가 대응하는 시그널 연산이 이루어질때까지 프로세스가 suspend 됨
  * x.signal(): suspend 되어있는 프로세스를 invoke 시킴



## 5.11 The Deadlock Problem

* The Deadlock Problem

  * 프로세스의 자원 사용시에 발생할 수 있는 문제
  * blocked 또는 waiting 상태의 프로세스들이 자원을 소유한채 다른 프로세스가 소유하고 있는 자원을 기다리고 있는 상태
  * 예시: 시스템에 프로세스와 디스크가 2개 있고 P1과 P2가 하나 씩 디스크를 사용하는 상태에서 상대편 디스크를 요청하는 경우

* Bridge Crossing Example

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image21.PNG?raw=true" alt="image" style="zoom:67%;" />

* Deadlock Example

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image22.PNG?raw=true" alt="image" style="zoom:67%;" />

* Deadlock Characterization

  * Deadlock이 발생하기 위한 4가지 조건
    * Mutual exclusion: 한 번에 한 프로세스만 사용할 수 있는 자원인 경우
    * Hold and wait: 프로세스가 최소한 하나의 자원을 소유한 상태에서 다른 프로세스의 자원 사용을 기다리는 경우
    * No preemption: 프로세스가 보유한 자원은 사용을 마친 후에 자발적으로 반납해야 하는 경우
    * Circular wait: 일련의 프로세스 간 서로의 자원을 요청하여 한 사이클을 형성하는 경우 

  

* Resource-Allocation Graph

  * Vertex의 집합 V와 Edge의 집합 E로 이루어짐

  * request edge: vertex P에서 vertex R로의 directed edge

  * assignment edge: vertex R에서 vertex P로의 directed edge

  * Resource-Allocation Graph (Cont.)

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image23.PNG?raw=true" alt="image" style="zoom:67%;" />

* Example of a Resource Allocation Graph (자원 할당 그래프 예시)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image24.PNG?raw=true" alt="image" style="zoom:67%;" />

* Resource Allocation Graph With A Deadlock

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image25.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 그래프 상의 사이클이 존재함

* Graph With A Cycle But No Deadlock

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap05/image26.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 그래프 상의 사이클이 존재하고 앞의 상황과 다르게 deadlock 상태가 아님



* Basic Facts
  * 자원 할당 그래프에서 사이클이 없다면 deadlock 상태가 아님
  * 그래프 상에서 사이클이 존재하는 경우
    * 자원의 instance가 하나만 있는 경우 -> cycle이 deadlock을 의미함
    * 자원의 instance가 여러개 있는 경우 -> deadlock의 가능성이 있음
* Methods for Handling Deadlocks
  * Deadlock을 발생하지 않도록 하는 방법
    * Deadlock prevention: deadlock의 필요조건 중에서 한가지 조건을 선택해서 해당 조건이 설립하지 않도록 관리
      * 프로세스의 많은 제약을 가해서 성능이 떨어짐
    * Deadlock avoidance: 시스템의 자원할당 상태를 구분함
      * safe: deadlock과 거리가 먼 정상적인 상태
      * unsafe: deadlock으로 발전할 수 있는 불안정한 상태
  * Deadlock을 탐지하면 이를 해결하는 방식
    * 대부분의 운영체제가 이에 해당함
