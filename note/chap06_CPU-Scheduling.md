# chapter 6 : CPU Scheduling

2021.04.27

<br>

## 6.1 Basic Concepts

* Basic Concepts

  * 프로세스의 실행은 CPU execution(실행)과 I/O wait로 이루어진다.
  * CPU burst: 프로세스가 CPU 실행을 이어나가는 구간, 이 장의 주요 내용
  * I/O burst: 프로세스가 입출력을 하는 구간

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image01.PNG?raw=true" alt="image" style="zoom: 67%;" />

* Histogram of CPU-burst Times

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image02.PNG?raw=true" alt="image" style="zoom: 67%;" />

  * 여러 프로세스들의 동작으로부터 CPU burst의 길이를 측정하여 길이의 분포를 나타낸 그래프
  * x축: CPU burst의 길이로 단위는 millisecond, y축: 빈도수

<br>

* CPU Scheduler

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image03.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Short-term scheduler: ready queue로 부터 다음번에 실행할 프로세스를 선택하는 동작을 함
    * ready queue에 프로세스들을 줄세우는 방식을 정하는 작업
  * CPU scheduling이 발생하는 지점
    1. Switches from running to waiting state: 현재 실행중인 프로세스가 입출력을 요청하는 경우
    2. Switches from running to ready state: 현재 실행중인 프로세스가 ready 상태로 돌아가는 경우
    3. Switches from waiting to ready: 새로 ready 상태가 되는 프로세스가 생기는 경우
       * 새로 ready 상태가 된 프로세스가 현재 running 상태의 프로세스보다 실행하기 더 적합하다면 실행이 전환될 수 있음
    4. Terminates: 현재 실행중인 프로세스가 종료하는 경우
  * nonpreemptive: 1번과 4번의 경우 (비선점형 scheduling)
    * 실행중인 프로세스가 자발적으로 CPU를 내놓는 경우
  * preemptive: 2번과 3번의 경우 (선점형 scheduling)
    * 운영체제가 running 프로세스로부터 CPU를 강제로 빼앗는 경우
    * 커널 내에서 preemption이 발생할 수 있고 interrupt 처리에 있어 설계에 세심하게 주의를 기울어야함

<br>

* Dispatcher
  * 단기 scheduler가 선택한 프로세스에게 CPU 제어를 넘겨주는 커널 내의 모델
    * switching context
    * switching to user mode
    * 사용자 프로그램의 적절한 위치로 분기하는 역할 수행
  * 모든 프로세스의 context switch 시에 호출되기 때문에 가능한 빨리 실행이 되어야 함
  * Dispatch latency: 하나의 프로세스를 정지시키고 다른 프로세스의 수행을 시작하는데 까지 걸리는 시간

<br>

## 6.2 Scheduling Criteria

1. CPU utilization: CPU 이용률로 CPU를 가능한 바쁘게 일하도록 유지하기를 원함
2. Throughput: 처리량으로 단위시간당 완료한 프로세스의 개수 (컴퓨터 생산성의 지표)
3. Turnaround time: 특정 프로세스의 관점에서 프로세스를 실행하는데 소요된 시간으로 프로세스의 시작시간과 완료시간의 간격
   * 메모리에 적재되기 위해서 기다린 시간, ready queue에 머무른 시간, CPU에서 실행한 시간, 입출력에 걸린 시간을 모두 합한 시간
4. Waiting time: ready queue 관점에서 프로세스가 ready queue에 진입해서 CPU를 사용할 때까지 대기한 시간
5. Response time: 하나의 요청을 시작한 후에 첫번째 응답이 나올때까지 걸린 시간, scheduling 성능의 중요한 척도

* Max: CPU utilization, Throughput
* Min: Turnaround time, Waiting time, Response time

<br>

## 6.3 Scheduling Algorithms

* Scheduling Algorithms
  * First-Come, First-Served (FCFS) Scheduling

    *  CPU를 먼저 요청하는 프로세스가 먼저 CPU를 할당받는 방식 (ready queue에 먼저 진입한 프로세스 순)

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image04.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Suppose that the processes arrive in the order: P1, P2, P3 

    * The Gantt Chart for the schedule

      <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image05.PNG?raw=true" alt="image" style="zoom: 50%;" />

    *  Waiting time for P1 = 0, P2 = 24, P3 = 27

    *  Average waiting time: (0 + 24 + 27) / 3 = 17

* FCFS Scheduling (Cont.)

  * Suppose that the processes arrive in the order: P2, P3, P1

    * The Gantt chart for the schedule is:

      <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image06.PNG?raw=true" alt="image" style="zoom: 50%;" />
      
    *  Waiting time for P1 = 6, P2 = 0, P3 = 3

    * Average waiting time: (6 + 0 + 3) / 3 = 3

  * 앞의 경우보다 대기시간이 대폭 개선됨

  *  Convoy effect: 하나의 긴 프로세스를 다른 프로세스들이 기다리는 경우

    * 긴 프로세스로 인해 CPU-bound와 여러 I/O-bound 프로세스간의 대기시간이 길어지고 Convoy effect가 일어날 수 있음


<br>

* Shortest-Job-First (SJF) Scheduling

  * ready queue 내의 프로세스 중에서 다음번 CPU burst가 가장 짧은 프로세스를 선택해서 scheduling 하는 방식
  * 프로세스의 전체 길이가 아닌 다음 CPU burst를 기준으로 함
  * 일련의 프로세스 집합에 대해서 평균 대기시간을 최소화한다는 점에서 최적의 scheduling 방식
    * 다음번 CPU burst 길이를 운영체제가 정확히 알 수 없다는 문제가 있음
    * 길이에 대해서 사용자(프로세스)에게 자신의 CPU burst를 제시하도록 하는 대안이 있음

* Example of SJF

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image07.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * SJF scheduling chart

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image08.PNG?raw=true" alt="image" style="zoom: 50%;" />
    
  * Average waiting time: (3 + 16 + 9 + 0) / 4 = 7

<br>

* Determining Length of Next CPU Burst (CPU burst 길이 추정)

  * 이전 CPU burst의 길이를 바탕으로 다음 CPU burst의 길이 예측

    * 프로세스별로 예측값을 각각 구한 다음 해당 예측값이 가장 짧은 순으로 scheduling 하게 됨

  * 지수 평균을 사용하여 다음 CPU burst를 구함

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image09.PNG?raw=true" alt="image" style="zoom: 50%;" />

    * t_n = n번째 CPU burst의 실제 길이
    * t_n+1 = 다음 CPU burst의 예측값
    * 보통 α는 ½로 설정

* Prediction of the Length of the Next CPU Burst

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image10.PNG?raw=true" alt="image" style="zoom: 50%;" />

<br>

* Examples of Exponential Averaging (CPU burst를 구하기 위한 지수평균의 예시)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image11.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * 다음번 CPU burst 예측값은 이전 CPU burst 예측값과 실제 측정값에 영향을 받는데 둘 중에 어느 것에 더 영향을 받는가는 알파 값에 따라 달라짐
  * 알파의 값에 따른 영향
    * 알파 = 0 : 직전 round의 실제 측정값이 반영되지 않음
    * 알파 = 1 : 직전 round의 예측값이 반영되지 않음
    * 지수평균의 식을 확장하면 뒷부분의 항들은 값이 작으므로 다음번 round의 추정값에 미치는 영향이 줄어드는 걸 알 수 있음

* Example of Shortest-remaining-time-first (선점형 scheduling)

  * 프로세스 별로 ready queue에 도착하는 시간이 각각 다른 경우 분석

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image12.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Preemptive SJF Gantt Chart

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image13.PNG?raw=true" alt="image" style="zoom: 50%;" />

  <br>

* Priority Scheduling (우선순위 Scheduling)

  * 각 프로세스마다 우선순위를 나타내는 정수를 부여함
  * 우선순위는 프로세스의 PCB에 저장이 되어있고 보통 정수값이 작을수록 우선순위가 높은 프로세스
  * ready queue의 프로세스 중에서 우선순위가 제일 높은 프로세스에 다음번 CPU를 부여하게 됨
  * Preemptive vs. Nonpreemptive (선점형 우선순위 Scheduling과 비선점형 우선순위 Scheduling)
    * 선점형 우선순위 Scheduling: 현재 실행중인 프로세스보다 우선순위가 높은 프로세스가 ready queue에 나타나면 즉시 실행하는 프로세스가 바뀌게 됨
    * 비선점형 우선순위 Scheduling: 우선순위에 따라서 일단 scheduling을 하고 scheduling 결과 CPU 사용을 시작하게 되면 자발적으로 프로세스가 CPU를 반납할때까지 실행을 이어나감
  * SJF 또한 일종의 우선순위 Scheduling에 해당하고 이 경우의 우선순위는 CPU burst 예측값의 역이 됨
  * 문제점: Starvation - 매번 높은 우선순위의 프로세스들이 ready queue에 들어와서 우선순위가 낮은 프로세스의 수행이 뒤로 밀리는 경우
  * 해결책: Aging - 우선순위가 낮은 프로세스여도 ready queue의 머무르는 시간이 늘어나면 우선순위를 높여주는 방법

* Example of Priority Scheduling

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image14.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Priority scheduling Gantt Chart

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image15.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Average waiting time = 8.2 msec

<br>

* Priority Inversion (우선순위 Scheduling을 사용할때 발생할 수 있는 문제 - 우선순위 역전 현상)

  * 우선순위 Scheduling을 하되 관련 프로세스들이 서로 자원을 사용하려하고 할때 발생할 수 있는 문제

  * CPU의 우선순위와 자원의 사용은 별개의 문제로 서로 얽히다보니까 우선순위 Scheduling의 취지에 벗어나는 현상

  * Priority Inversion의 실제 예시 (우선순위 역전 예시) - 우선순위가 다른 세 개의 프로세스 Th, Tm, Tl

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image16.PNG?raw=true" alt="image" style="zoom: 50%;" />

    1. 처음에 Tl이 실행을 시작함
    2. Tl이 semaphore(자원의 예시, 독점적으로 사용할 수 있는 자원)을 얻게 됨
    3. Tl이 semaphore을 소유하면서 실행하는 구간
    4. Tl보다 우선순위가 높은 Th가 ready queue에 나타나서 실행이 Th로 바뀜
    5. Th가 실행하는 구간으로 Tl은 semaphore을 보유하면서 ready queue로 돌아감
    6. Th가 Tl의 semaphore를 얻고 싶어하여 Th는 waiting 상태로 바뀌게됨
    7. ready queue에 머물고 있던 Tl이 다시 실행됨
    8. ready queue에 Tm이 들어어고 우선순위가 Tl보다 높기 때문에 Tm이 실행됨
    9. Tl은 다시 ready queue에 들어가고 Tm이 실행되는 구간
    10. Tm의 실행이 끝나서 ready queue에 있는 Tl이 다시 실행됨
    11. Tl이 실행되는 구간
    12. Tl이 사용한 자원인 semaphore을 release 하고 Th의 우선순위가 더 높기 때문에 Th가 실행되고 Tl은 ready queue로 들어감
    13. Th가 semaphore를 얻고 실행이 이어나가는 구간

<br>

* Priority Inheritance (Donation) Protocol (우선순위 역전 현상에 대한 해결책)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image17.PNG?raw=true" alt="image" style="zoom: 50%;" />

  1. 처음에 Tl이 실행을 시작함
  2. Tl이 semaphore(자원의 예시, 독점적으로 사용할 수 있는 자원)을 얻게 됨
  3. Tl이 semaphore을 소유하면서 실행하는 구간
  4. Tl보다 우선순위가 높은 Th가 ready queue에 나타나서 실행이 Th로 바뀜
  5. Th가 실행하는 구간으로 Tl은 semaphore을 보유하면서 ready queue로 돌아감
  6. Th가 Tl의 semaphore를 얻고 싶어하여 Th는 waiting 상태로 바뀌게되고 높은 우선순위의 Th가 요구하는 semaphore를 낮은 우선순위인 Tl이 갖고있으므로 Tl의 우선순위를 임시로 Th와 같게 올려줌
  7. ready queue에 머물고 있던 Tl이 임시로 우선순위가 높여진 상태로 다시 실행됨
  8. Tl이 semaphore의 사용을 마쳐 release 하면 Th가 다시 ready 상태로 바뀌고 Tl의 우선순위는 다시 원래대로 낮은 우선순위로 바뀜
  9. Th가 semaphore를 얻은 상태로 실행을 이어나감
  10. Th의 실행이 끝나고 ready queue에 있는 Tm이 다시 실행됨
  11. Tm이 실행되는 구간

<br>

* Round Robin (RR) Scheduling

  * 프로세스가 ready queue에 도착한 순서대로 실행을 하게 되는데 이때 미리 정해진 time quantum q 만큼 실행을 한 후에는 CPU를 빼앗기게 됨
  * time quantum: 한번 실행을 이어나갈 수 있는 최대 시간
  * ready queue에 n개의 프로세스가 있고 time quantum이 q라고 가정을 하면 각 프로세스는 최대 q만큼의 시간단위로 전체 CPU 시간의 1/n을 얻게 됨
  * 각 프로세스는 자신의 다음 time quantum이 할당될 때까지 (n-1) * q 시간 이상은 대기하지 않게 됨
  * Timer interrupts를 이용하면 매 time quantum이 경과할때마다 다음 프로세스를 scheduling 할 수 있음
  * 성능: time quantum을 어떻게 설정하느냐에 따라 달라짐
    * q의 값이 큰 경우 -> FIFO와 마찬가지 결과
    * q의 값이 작은 경우 -> context switch에 들어가는 overhead 가 커져서 실제로 동작하는 시간보다 context switch에 소모하는 시간이 더 많아짐

* Example of RR with Time Quantum = 4

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image18.PNG?raw=true" alt="image" style="zoom: 50%;" />

<br>

* Time Quantum and Context Switch Time

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image19.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * time quantum 시간 q 설정: 10ms ~ 100ms
  * 보통 context switch 에 걸리는 시간 <10 usec

* Turnaround Time Varies With The Time Quantum (time quantum에 따른 평균 turnaround time)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image20.PNG?raw=true" alt="image" style="zoom: 50%;" />

  <br>

* Multilevel Queue

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image21.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * 시스템 내의 프로세스들이 몇개의 그룹으로 분류될 수 있는 경우에 ready queue를 나누어서 운영할 수 있음
    * foreground (interactive) - 응답시간이 짧은 것을 선호
    * background (batch) - 응답시간이 긴 것을 선호
  * 각 프로세스가 ready queue에 진입할 때마다 미리 정해진 queue에만 머무르게 됨
  * 같은 queue에 머무르는 성격이 비슷한 프로세스들 끼리도 별도의 scheduling 알고리즘이 있어서 같은 queue에 머무르는 프로세스 간에는 지정된 scheduling 알고리즘을 적용함
    * foreground - RR, background - FCFS
  * queue 들간에도 scheduling도 필요한데 queue들간의 scheduling은 전형적으로 고정 우선순위의 선점형 scheduling을 주로 사용
    * foreground queue와 background queue에 각각 프로세스가 있으면 foreground queue의 프로세스들이 우선순위가 높아서 우선적으로 scheduling 되고 background의 프로세스는 foreground queue에 프로세스가 없는 경우에만 서비스가 됨
    * background가 서비스 되는 도중에 foreground queue에 ready process가 생기게 되면 CPU 교체 선점이 발생하도록 함
    * background queue에 있는 프로세스는 starvation 가능성이 있으며 이를 방지하려면 전체 CPU 시간을 적절한 비율로 나눠서 배분하는 방법이 있음

<br>

* Multilevel Feedback Queue

  * 프로세스가 queue 간에 이동하는 것을 허용하는 경우
  * Multilevel Feedback Queue의 parameters
    * number of queues: queue의 개수
    * scheduling algorithms for each queue: queue에서 사용할 scheduling 알고리즘
    * method used to determine when to upgrade a process: 언제 프로세스를 높은 우선순위 큐로 올려보낼지 결정
    * method used to determine when to demote a process: 언제 프로세스를 낮은 우선순위 큐로 올려보낼지 결정
    * method used to determine which queue a process will enter when t hat process needs service: 프로세스가 큐에 진입할때 시작할 큐를 정해야함 

* Example of Multilevel Feedback Queue

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image22.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * 3개의 queue
    * Q0 – RR with time quantum 8 milliseconds
    * Q1 – RR time quantum 16 milliseconds
    * Q2 – FCFS
  * Scheduling
    * 새로운 프로세스는 Q0에서 FCFS로 시작함
      *  프로세스가 CPU를 갖게되면 8 milliseconds로 동작함
      * 8 milliseconds를 모두 사용하지 않고 입출력을 하게된다면 프로세스는 다음번에 ready queue에 진입할 때 Q1으로 이동
    * Q1에서는 다시 FCFS를 수신하고 16 milliseconds를 추가로 동작함
      * 만약 동작이 완료되지 않으면 Q2로 이동

<br>

## 6.6 Real-Time CPU Scheduling

* Real-Time CPU Scheduling

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image23.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Real-time system은 event-driven 방식으로 동작함 
    * Soft real-time systems vs. Hard  real-time systems (마감시간의 수행이 엄격하지 않은 시스템과 엄격한 시스템)
  * Event latency: Event가 발생하여 그에 대한 적절한 서비스가 수행이 될때까지 걸리는 시간 (간격)
    * 사건에 연관된 dead line 보다 짧아야함
  * 성능에 영향을 주는 두가지 종류의 latencies
    * Interrupt latency: CPU의 event에 해당하는 interrupt가 도착한 시점으로부터 해당 interrupt의 처리 루틴이 실행을 시작하기까지 걸리는 시간
    * Dispatch latency: scheduling dispatcher가 한 프로세스를 중지시키고 다른 프로세스를 시작시키는데 걸리는 시간

<br>

* Real-Time CPU Scheduling (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image24.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * dispatch latency의 구성요소
    * conflicts: 커널 모드에서 동작하고있는 프로세스에 대한 선점
    * dispatch: 높은 우선순위의 프로세스가 필요로하는 자원을 낮은 프로세스가 release하는 동작

<br>

* Priority-based Scheduling

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image25.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * real-time scheduling에서 실시간 프로세스가 CPU를 필요로 할때 이에 즉각 응답을 해주는 기능이 가장 중요함
  * 실시간 운영체제의 scheduler는 선점을 이용한 우선순위 기반의 scheduling 알고리즘을 제공해야함
    * 선점형 우선순위 기반 scheduling은 soft real-time 기능을 제공하는 것에 불과함
  * hard real-time system에서는 실시간 task가 주어진 dealine 내에 정확히 수행되는 것을 보장해야함
  * 각 task(실시간 프로세스) 들이 periodic(주기적)한 특징을 갖고있음
    * 일정한 수행시간: t, deadline: d, period(주기): p
    * 0 <= t <= d <= p
    * periodic task의 rate: 1/p

<br>

* Rate Montonic Scheduling (Real-Time Scheduling 중 하나)

  * 우선순위 Scheduling으로 선점형, static(우선순위 값이 변하지 않음) 함

  *  Shorter periods = higher priority, Longer periods = lower priority

  *  P1의 우선순위가 P2보다 높은 경우 (P1 = 20, P2 = 35)

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image26.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Missed Deadlines with Rate Monotonic  Scheduling (프로세스가 갖고있는 deadline 요구사항을 지킬 수 없는 경우)

  * P1 = 25, P2 = 35인 경우로 25/50 + 35/80 = 0.94

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image27.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Earliest Deadline First Scheduling (EDF)

  * 마감시간에 따라 우선순위를 동적으로 부여하는 기법
    * 새로 실행가능하게 된 프로세스의 마감시간에 맞춰서 우선순위가 다시 조정됨

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image28.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Proportional Share Scheduling

  * 모든 응용(application)들에게 T개의 share을 할당하여 동작함
  * 한개의 응용(application)이 N개의 몫을 받을 때 N < T
  * 응용(application)은 N / T의 시간을 할당받게 됨



## 6.7 Operating System Examples

* Operating System Examples

  * Linux scheduling
  * Windows scheduling

* Linux Scheduling Through Version 2.5

  * kernel version 2.5까지는 기존의 UNIX scheduling 방식을 변형하여 사용함
  * Version 2.5까지의 scheduling 기법에서 특징중 하나는 task 개수와 무관하게 상수시간 O(1) 이 scheduling 시간을 결정함
  * task를 time-sharing과 real-time 프로세스로 나누고 priority도 다르게 부여함
  * priority를 나타내는 숫자가 낮을수록 높은 우선순위를 갖는것으로 가정을 했고 높은 우선순위를 갖을수록 CPU를 좀 더 많이 사용할 수 있는 권리를 얻음

* Linux Scheduling in Version 2.6.23 +

  * Completely Fair Scheduler (CFS): Scheduling classes를 기반으로 둠
  * Scheduling classes
    * 각각의 우선순위가 부여되어 있고 scheduler는 가장 우선순위의 클래스와 프로세스를 선택함
    * 기본적인 클래스로는 default와 real-time 클래스가 있음
  * 길이가 일정한 (고정된) time quantum을 사용하는 대신에 각 task의 CPU time의 비율을 할당함
    * time quantum은 nice value를 통해 계산되고 이 값이 낮을수록 우선순위가 높아짐
    * Target latency: 다른 모든 수행가능한 task가 적어도 한번씩은 실행할 수 있는 시간 간격
  * CFS에서는 각 task 별로 virtual run time 이라는 변수가 있어서 해당 task가 실행된 시간을 기록하여 virtual runtime을 유지하게 됨
    * virtual runtime: task의 우선순위에 기반을 둔 decay rate와 연관이 있음
    * 보통 우선순위의 task는 virtual runtime이 물리적인 실행시간과 같지만 낮은 우선순위의 task는 실제 실행한 시간보다 virtual run이 크다
  * 다음에 실행할 task로 가장 작은 virtual run time을 갖는 task를 선택함

* CFS Performance

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image29.PNG?raw=true" alt="image" style="zoom: 50%;" />

  <br>

* Windows Scheduling (Dispatcher)

  * 우선순위를 기반으로 하는 preemptive scheduling을 기반으로 함
  * 가장 높은 우선순위의 thread가 실행됨
  * Windows Scheduling이 실행되는 경우
    1. blocks: 입출력하는 경우
    2. uses time slice: 주어진 것을 사용하는 경우
    3. preempted by higher-priority thread: 우선순위가 높은 task에게 선점되는 경우
  * 우선순위가 32단계로 구분됨
    * Variable class: 1~15
    * real-time: 16~31
    * memory-management thread: 0
  * 각 Priority level 마다 queue가 존재하고 프로세스는 각 레벨에 맞는 priority queue에 들어가서 ready 상태에 머무름

* Windows Priority Classes

  * priority class (6개)
    *  REALTIME_PRIORITY_CLASS, HIGH_PRIORITY_CLASS, ABOVE_NORMAL_PRIORITY_CLASS,NORMAL_PRIORITY_CLASS, BELOW_NORMAL_PRIORITY_CLASS,  IDLE_PRIORITY_CLASS
  * relative priority (7개)
    *  TIME_CRITICAL, HIGHEST, ABOVE_NORMAL, NORMAL, BELOW_NORMAL, LOWEST, IDLE
  * priority class와 relative priority를 합치면 42개의 numeric priority가 만들어질 수 있음
  * Base priority는 NORMAL이고 time quantum이 완료되면 priority가 낮아지지만 base 이하로 낮아지지 않음

* Windows Priorities

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap06/image30.PNG?raw=true" alt="image" style="zoom: 50%;" />