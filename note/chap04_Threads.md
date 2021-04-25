# chapter 4 : Threads

2021.03.30

```markdown
* 주요내용
1. Thread에 대한 Overview
2. Multicore Programming에 대한 소개
3. Multithreading이 운영체제가 지원하고 사용할때 필요한 Multithreading Models에 대해 알아봄
4. Thread 사용을 도와주는 Thread Libraires에 대해 알아봄
5. Thread를 만들고 관리하는 일을 대신해주는 작업인 Implicit Threading에 대해 알아봄
6. Threading Issues
```



## 4.1 Overview

* Overview

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image01.PNG?raw=true" alt="image" style="zoom:67%;" />

  * 하나의 프로세스는 기본적으로 메모리상의 이미지인 code, data, stack 등으로 구성됨
  * 실행중인 프로세스인 경우에는 Program Counter를 비롯한 각종 registers와 프로세스 자원에 해당하는 file들도 갖게됨
  * 프로그램을 3번 실행시킨 결과는 위와 같은 3개의 독립적인 프로세스가 생성됨 (single-threaded process)
  * 프로세스를 1개로 합치게 되면 code는 항상 같으며 data, files는 바뀌지만 공유가 가능한 값으로 code, data, files를 1개를 서로 공유하고 register, stack은 공유가 불가능하므로 각각 1개씩 갖고 있음 (multi-threaded process)
  * Thread: 실행시 마다 달라지는 부분으로 공유가 불가능하며 register, stack을 합한 개념
  * Multi-threaded를 지원하는 운영체제에서 실행 기본단위는 threaded이고 스케줄링의 대상도 전체가 아닌 개별 threaded가 됨

* Motivation

  * 현재 대부분의 응용들은 multi-threaded 방식으로 동작하고 하나의 응용은 여러개의 독립적인 프로세스들로 구현됨
  * 하나의 응용이 수행해야하는 여러 개의 기능들을 개별 thread로 구현할 수 있음
    * Update display, Fetch data, Spell checking, Answer a network request 등
  * 프로세스 하나를 생성하는데 많은 비용이 필요하지만 thread 하나를 생성하기는 간단하여 light-weight 프로세스라고도 함
  * multi-threaded 프로세스를 사용하면 코드 구성을 단순화할 수 있고 실행의 효율을 증가시킬 수 있음
  * 현대의 운영체제는 kernel이 여러 개의 thread로 이루어져 있음

* Multi-threaded Server Architecture - Multi-threaded가 사용되는 전형적인 예시

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image02.PNG?raw=true" alt="image" style="zoom:67%;" />

  1. request: client로부터 server로 서비스 요청이 들어옴
  2. create new thread to service the request: 요청을 처리할 수 있는 서비스 thread를 생성하여 처리를 넘김
  3. resume listening for additional client requests: 서버는 또 다른 클라이언트 요청을 대기하는 구조

* Benefits (Multi-threaded 장점)

  * Responsiveness - 운영체제의 일부 프로세스가 blocked 되거나 긴 작업을 수행하더라도 프로그램의 다른 부분은 수행이 계속되는 것을 허용하기 때문에 사용자에 대한 응답성을 향상할 수 있음
  * Resource Sharing - thread는 자신이 속한 프로세스 내의 다른 thread 들과 메모리, 각종 자원들을 공유할 수 있음
  * Economy - thread는 자원을 공유하기 때문에 프로세스를 계속해서 생성하는 것보다 경제적임
  * Scalability - 각 thread가 다른 처리기에서 병렬적으로 실행이 될 수 있어서 성능 향상을 기대할 수 있음



## 4.2 Multicore Programming

* Multicore or multiprocessor systems

  ```markdown
  Multiprocessor systems: SMP와 같이 여러개의 CPU를 갖고 있는 시스템
  Multicore systems: 단일 CPU 칩 안에 명령을 실행할 수 있는 단위인 core가 여러 개인 시스템
  ```

  * Parallelism (병렬 실행)

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image03.PNG?raw=true" alt="image" style="zoom:67%;" />

    * 여러 개의 task를 동시에 실행시킬 수 있음
    * 그림은 core 2개가 task 4개를 Parallelism하는 경우

  * Concurrency (병행 실행)

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image04.PNG?raw=true" alt="image" style="zoom:67%;" />
    
    * 여러 개의 task가 동시 진행이 됨 CPU가 1개인 경우도 가능함
    * Multi Programming, Multi Tasking, Time Sharing이 추구하는 기법
    * 그림은 core 1개가 task 4개를 Concurrency하는 경우

* Multicore Programming (Cont.)

  * parallelism의 유형
    * Data parallelism: 하나의 데이터 집합의 부분집합을 다수의 컴퓨팅 core에 분배해서 각 core가 동일한 연산을 실행하는 경우
    * Task parallelism: thread가 각각 다른 작업을 하는 경우로 이들을 다수의 core에 분배하여 실행시킴
  * 다중 코어 시스템에서 프로그래밍을 하기위한 과제
    * Dividing activities: 응용을 분석하여 독립된 병행가능한 task로 나눌 수 있는 영역을 찾는 과정이 필요함
    * Balance: 각 부분이 전체 작업에 균등한 기여도를 갖도록 나누는 과정
    * Data splitting: 응용이 독립된 task로 나누는 것처럼 task가 접근하는 데이터 또한 개별 core에서 사용할 수 있도록 나누는 과정
    * Data dependency: task가 접근하는 데이터가 서로간의 종속성이 없는지 검토하는 과정
    * Testing and debugging: 다양한 실행경로가 존재할 수 있으므로 디버깅에 주의해야함




* Amdahl’s Law - 병렬 실행의 성능에 관한 법칙

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image05.PNG?raw=true" alt="image"  />
  
  * 순차 실행을 해야하는 구성요소와 병렬 실행이 가능한 요소를 동시에 갖고 있는 응용에 대해서 core의 개수를 증가시켰을 때 얻을 수 있는 성능 향상을 나타내는 공식
  * S는 순차실행을 해야하는 비율, N은 core의 개수
  * N이 커지면(core의 개수가 많아지면) 성능은 향상되지만 1/S로 수렴하게 된다.
  * 응용에 포함된 순차실행은 core를 추가하여 얻을 수 있는 성능향상에 불균형적인 영향을 미치게 됨



## 4.3 Multithreading models

* Multithreading models

  * User threads: 사용자 수준에서 (유저 모드에서) 동작하는 thread로 thread library를 통해서 사용

  * 대표적인 thread libraries: POSIX Pthreads, Windows thread, Java threads

  * Kernel threads: Kernel 수준에서 제공하는 thread

  * 범용 운영체제는 Kernel thread를 지원함 (Windows, Linux, Mac OS X, Solaris)

  * model 종류

    * Many -to -One

      <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image06.PNG?raw=true" alt="image" style="zoom: 67%;" />

      * 여러 개의 사용자 수준 thread가 하나의 커널 thread에 연관된 경우
      * 다수의 user thread 중 하나가 blocking 시스템콜을 하면 유일한 커널 thread가 blocked 상태가 되면서 나머지 thread도 blocked 됨
      * 한 번에 하나의 thread가 커널에 접근할 수 있기 때문에 여러 thread가 다중 core 시스템에서 병렬로 실행될 수 없음
      * 한 번에 한 thread만 수행가능한 형태

    * One-to-One

      <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image07.PNG?raw=true" alt="image" style="zoom: 67%;" />

      * 각 user-level thread가 하나의 커널 thread와 일대일 관계를 이루는 경우
      * user-level thread를 하나 만들면 그에 대응하는 커널 thread가 생성됨
      * many-to-one 보다 훨씬 더 많은 concurrency를 제공
      * 너무 많은 커널 thread가 생성될 수 있어서 대부분의 시스템에서 process가 thread 개수를 제한함
      * Windows, Linux, Solaris 9 and later 등이 사용

    

    * Many-to-Many Model

      <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image08.PNG?raw=true" alt="image" style="zoom: 67%;" />

      * 다수의 사용자 thread를 복수개의 커널 thread에 연관된 경우
      * 여러개의 사용자 thread를 그거보다 작거나 같은 수의 커널 thread로 multiplexing 하는 방식
      * 커널이 적절한 수의 커널 thread를 생성해두고 사용자 thread가 생길 때마다 대응시키는 방식
      * Windows, Solaris9의 ThreadFiber package에 사용됨

    * Two-level Model

      <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image09.PNG?raw=true" alt="image" style="zoom: 67%;" />
      
      * 시스템 내에서 many-to-many와 one-to-one을 모두 사용하는 방식
      * 과거의 일부 UNIX 계열 운영체제에서 사용



## 4.4 Thread Librarie

* Thread Libraries

  * 프로그래머에게 Thread를 생성하고 관리할 수 있는 API를 제공함
  * 구현하는 방법(2가지)
    * User level 에서만 구현
    * 운영체제가 지원하는 커널 수준의 라이브러리도 있음
  * POSIX Pthreads, Windows Threads, Java Thread에 대해 알아봄

* Pthreads

  * 대표적으로 널리 사용되는 thread 라이브러리로 POSIX 표준의 일부
  * Specification(명세)만 정의하고 있을 뿐 implementation(구현) 방법은 제공하지 않음
  * UNIX 계열 운영체제에서 제공함 (Solarix, Linux, Mac OS X)

* Pthreads Example

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image10.PNG?raw=true" alt="image" style="zoom: 67%;" />

  * 1 ~ n 까지의 정수의 합을 구하는 프로그램
  * 프로그램 구성
    1. 헤더파일: Pthread 라이브러리 함수를 사용하기 위해 정의
    2. main 함수: 숫자 n의 입력, thread의 생성, 최종 결과값을 출력
    3. runner 함수: 별도의 thread로 동작
    4. 전역변수 sum: 결과값을 담을 전역변수로 thread 사이에 공유됨

* Windows Multithreaded C Program

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image11.PNG?raw=true" alt="image" style="zoom: 67%;" />

  1. 헤더파일 - `#include <windows.h>`
  2. 전역변수 - `DWORD Sum;`
  3. 숫자의 합을 계산하기 위한 thread로 동작하는 함수 - `Summation`
  4. main 함수: 숫자 n의 입력, thread의 생성, 최종 결과값을 출력



## 4.5 Implicit Threading

* Implicit Threading 정의

  * 프로그램이 많은 수의 thread로 구성되는 경우 문제가 발생하지 않게 thread의 생성과 관리를 프로그래머가 직접하지 않고 컴파일러나 런타임 라이브러리가 대신하는 것을 말함

* 3가지 방법
  * Thread Pools

    * thread의 개수가 무한정 늘어나는 것을 방지하는 기법
    * process를 시작할 때 일정한 수의 thread를 pool 개념으로 만드는 방식
    * 장점
      * 미리 thread를 만들어서 필요시에 thread를 만드는 경우보다 동작이 빠름
      * 한 응용을 위한 thread의 개수를 pool의 크기로 제한할 수 있음
      * task의 실행과 생성을 분리하여 실행에 다양한 전략을 적용할 수 있음
    * Windows API는 thread pool을 지원함

  * OpenMP

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image12.PNG?raw=true" alt="image" style="zoom: 67%;" />

    * C, C++, FORTRAN을 위한 컴파일러 directives와 API의 집합
    * 공유메모리 환경의 병렬 프로그램을 지원하고 병렬로 실행가능한 부분을 parallel regions 이라고 부름
    * 개발자는 자신의 코드 중에서 parallel regions에 컴파일러 directory를 삽입하여 OpenMP 가 해당 영역을 병렬로 실행하도록 할 수 있음

  * Grand Central Dispatch

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap04/image13.PNG?raw=true" alt="image"  />

    * Mac OS를 위해 개발된 기술로 C, C++, API, run-time library를 확장한 기술
    * Block을 정의할 수 있음 - "^{}"
    * Block을 dispatch queue에 넣어서 실행이 될 수 있도록 schedule하게 됨
    * dispatch queues (2가지 유형)
      * serial: FIFO 방식으로 queue에서 꺼내진 내용은 다른 block을 꺼내기 전에 실행이 되도록 함 (main queue)
      * concurrent: FIFO 방식으로 한번에 여러 개를 꺼내고 실행할 수 있음
        * priorities low, default, high

* Threading Issues

  * thread가 여러개 포함되면서 fork()와 exec()의 의미가 달라질 수 있음
  * Signal handling: 프로세스에게 가는 신호인 Signal을 어떤 thread가 받아야하는지 정의가 필요함
  * Thread cancellation: thread를 종료시키는 문제
  * Thread-local storage: thread 별로 local storage를 갖는 문제
  * Scheduler Activations



