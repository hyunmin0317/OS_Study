# chapter 1 : Introduction

2021.03.02

1. 운영체제를 이해하는데 필요한 컴퓨터 시스템의 기초지식
2. 운영체제의 기능과 주요 구성 요소
3. 운영체제에 큰 영향을 미치는 다양한 컴퓨터 환경



* Contents
  * 운영체제가 어떤 소프트웨어인지와 운영체제가 하는 일을 설명
  * 운영체제는 컴퓨터 하드웨어와 가장 밀접한 관련이 있는 소프트웨어이므로 운영체제를 이해하기 위해서는 하드웨어에 대한 기본적인 지식이 필요함
  * Multi Programming - 운영체제에서 가장 중요한 개념
  * 기본적인 운영체제의 구조와 동작 방식
  * 운영체제의 주요 기능 : 프로세스 관리, 메모리 관리, 저장장치 관리, 보호 및 보안
  * 컴퓨터가 사용되는 다양한 컴퓨팅 환경
  * 주요한 오픈 소스 운영체제



### 1.1 What Operating Systems Do

---

운영체제가 무엇인지와 어떤 일을 하는지에 대해서 설명



* 사용자 관점에서의 운영체제
  * 개인용 컴퓨터 (PC)
    * 한 사용자가 단독으로 사용하기에 적합하도록 최적화 되어 있음
    * 사용자가 자원들을 독점하도록 설계되어 있으며 사용의 편의성을 제공하는 것이 중요함
    * 자원 사용의 효율성에는 신경을 쓰지 않음
  * 대형 컴퓨터 (mainframe, minicomputer)
    * 여러명의 사용자가 하나의 컴퓨터를 공유하므로 자원의 이용을 극대화하도록 설계되어 있음
    * 모든 가용 CPU 시간, 메모리, 입출력 장치들이 효율적으로 관리되어 개인은 주어진 몫만 사용가능
  * 네트워크를 통해서 서버에 연결된 workstation
    * 자신이 마음대로 사용가능한 전용 자원도 가지고 있지만 파일, 프린터 서버는 여러 사용자가 공유
  * 휴대용 컴퓨터 (스마트폰, 태블릿)
    * 대부분 개인 사용자를 위한 독립형 장치로 일반적인 컴퓨터에 비해 제한된 성능의 하드웨어를 갖고 있으며 배터리로 동작을 하는 등 전혀 다른 컴퓨터 환경을 제공하므로 운영체제도 그에 따라 다름
  * 내장형 컴퓨터 (가전제품, 자동차)
    * 별도의 사용자 인터페이스가 없거나 제한적인 인터페이스를 갖고 있으며 사용자의 개입없이 작동할 수 있도록 설계되어야 함



* 시스템 관점에서의 운영체제
  * 운영체제(OS)는 하드웨어와 가장 밀접하게 연관되어 있는 프로그램
  * 시스템 관점에서 OS는 자원 할당자 (resource allocator) 라고 할 수 있음
    * 컴퓨터는 CPU, 메모리, 각종 입출력 장치등 운영프로그램이 문제 해결을 위해 필요로 하는 하드웨어 자원으로 이루어져있고 OS는 이러한 자원에 대한 관리자로 요청에 따라 필요한 자원을 할당하는 역할을 함
  * 시스템 관점에서 OS는 제어 프로그램 (control program) 이라고 할 수 있음
    * 운영체제는 프로그램을 실행시키고 실행중인 운영 프로그램의 실행을 제어하는 역할을 함



* 운영체제 (Operating System) 정의
  * 새로운 컴퓨터에 운영체제를 설치하는 경우에 운영체제를 설치한 직후 컴퓨터 시스템에 담긴 모든 것
  * 전체 운영체제 중에 컴퓨터 동작하는 중 항상 실행되는 부분은 커널이고 패키지에 포함된 내용 중에 커널에 포함되지 않은 것은 시스템 프로그램이며 시스템의 작동과 관련없는 나머지를 응용 프로그램이라고 함
  * 운영체제는 컴퓨터 사용자와 하드웨어 사이에서 중간 역할을 해주는 프로그램
  * 운영체제의 목표 : 사용자 프로그램을 실행하고 컴퓨터 시스템을 편리하게 사용할 수 있도록 해주며 컴퓨터 하드웨어가 효율적으로 사용되도록 하는 것을 목표로 함



### 1.2 Computer System Structure (컴퓨터 시스템의 구성)

---

운영체제를 이해하기 필요한 기초 지식



* 컴퓨터 시스템의 4가지 구성요소

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image01.PNG?raw=true" alt="image01" style="zoom:80%;" />

  1. 하드웨어
     * 기본적인 컴퓨팅 자원
     * CPU, memory, 다수의 입출력 장치로 구성
  2. 운영체제
     * 시스템 소프트웨어로서 응용 프로그램이 하드웨어 자원을 사용하는 것을 통제하는 역할 
  3. 응용 프로그램
     * 사용자의 문제 해결을 돕는 소프트웨어로서 하드웨어 자원을 사용하는 주체
     * 워드 프로세서, 웹 브라우저, 게임 소프트웨어 - 일상 생활에서 사용하는 프로그램
  4. 사용자
     * 컴퓨터를 사용하는 사람이나 또 다른 컴퓨터



* Computer System Organization (컴퓨터 하드웨어)

  * 컴퓨터 하드웨어의 구성

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image02.PNG?raw=true" alt="image02" style="zoom:80%;" />

    * 컴퓨터 하드웨어에서 CPU와 메인메모리를 제외한 나머지 요소들은 모두 입출력 장치임
    * CPU : 프로그램 또는 프로그램을 이루고 있는 명령들을 실행하는 장치
    * 메인 메모리 : 프로그램이 실행되는 동안 이를 저장하는 장치
    * 입출력 장치 : 컴퓨터 외부와 데이터를 주고 받거나 데이터를 보관하는 장치



* Computer System Operation (컴퓨터의 동작 과정)
  * CPU와 입출력 장치의 장치 컨트롤러는 버스 (데이터 통로)를 통해 메모리에 연결되어 있음
  * CPU와 입출력 장치는 동작하며 메모리와 데이터를 주고 받아야 함
  * CPU와 입출력 장치는 서로 독립적으로 작동함
  * 보통은 입출력 장치마다 해당 장치의 동작을 제어하는 컨트롤러가 있음 
  * 컨트롤러는 입출력중인 데이터를 임시로 보관하는 로컬 버퍼를 갖고 있음
  * CPU는 메인 메모리와 레지스터 간의 데이터를 이동시키는 동작을 함
  * 입출력 장치는 메인 메모리와 로컬 버퍼 (입출력 장치의 레지스터) 간의 데이터를 이동시키는 동작을 함
  * CPU와 장치 컨트롤러는 서로 독립적으로 동작하지만 때로는 동작의 동기화가 필요한 경우가 있음
  * 입출력 장치의 입출력이 완료된 경우 이를 CPU에게 interrupt를 통해 알림



* Common Functions of Interrupts
  * CPU는 동작하는 중에 입출력 장치로부터 interrupt 신호를 받으면 하던 일을 멈추고 해당 interrupt를 처리하는 interrupt 서비스 루틴으로 제어를 옮겨 이를 실행함
  * interrupt vector는 위 과정에서 사용되며 interrupt의 유형을 나타내는 고유 숫자임
  * 운영체제는 interrupt vector 값에 대해 해당 interrupt를 처리하는 함수의 시작 주소를 갖고 있음
  * interrupt 처리함수는 interrupt 발생 시 CPU가 처리해야하는 동작을 미리 정의해둔 함수
  * interrupt의 종류
    * 하드웨어 interrupt (외부 interrupt) : 발생 원인이 CPU 외부인 입출력 장치인 경우
    * trap, exception (내부 interrupt) : CPU가 명령을 실행시키는 과정에서 발생하는 interrupt
  * 운영체제는 interrupt driven 방식으로 동작함



* Interrupt Handling
  * OS는 interrupt가 발생 시에 그 시점의 CPU 상태를 (CPU 레지스터 값) 저장해 놓음
  * CPU는 각 장치의 interrupt를 구분해서 처리해야 함
    * polling : 각 입출력 장치마다 interrupt를 보냈는지 여부를 확인하여 장치를 구분
    * vectored interrupt system : 장치에서 전달된 vector 번호로 장치를 구분
  * interrupt를 발생한 장치를 확인했으면 CPU는 상태 저장을 거쳐서 interrupt 처리함수를 실행



* Interrupt Timeline

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image03.PNG?raw=true" alt="image03" style="zoom:67%;" />

  1. 입출력 개시 요청
     * 실행중인 운영 프로그램이 입출력 명령을 실행하면 CPU로부터 입출력 장치로 입출력을 시작하라는 지시가 전달됨
  2. 입출력이 시작된 이후의 CPU와 입출력 장치의 상태를 표시
     * CPU는 입출력 시작을 지시한 후 자신의 일을 하고 입출력 장치는 지시받은 입출력 동작을 수행함
  3. 입출력의 완료에 의해 interrupt 발생
     * 입출력이 완료되어 입출력 장치가 CPU에게 interrupt를 보내 이를 알림
     * vector 방식은 interrupt의 vector 번호가 보내짐
     * polling 방식은 CPU가 어떤 장치의 interrupt인지 찾아내는 과정 필요
  4. interrupt를 보낸 장치가 확인 된 후 이에 대한 interrupt 처리 함수가 실행
     * interrupt 처리함수인 운영체제 커널이 실행됨
  5. 입출력 완료
     * 입출력 장치는 입출력을 마치고 완료 interrupt를 보낸 뒤에 다음 지시까지 아무일도 하지 않음 (idle)
     *  CPU는 interrupt 처리가 끝나고 응용 프로그램으로 돌아가 발생 직전의 일을 계속함



* Storage Structure
  * 메인 메모리 (Main memory) : CPU가 직접 접근할 수 있는 유일한 저장장치, D-Ram 
    * Random access : 데이터가 저장장치 내에 저장된 위치와 관계없이 일정한 속도로 접근 가능
    * Typically volatile (휘발성) : 컴퓨터 전원이 꺼지면 저장된 데이터가 모두 사라짐
  * 보조 저장장치 (Secondary storage) : 비휘발성 메모리로 큰 저장용량을 갖는 저장장치
    * 마그네틱 디스크 (Magnetic disk, 하드디스크)
      * 원형의 금속판에 자석 물질을 입힌 후 그 위에 데이터를 저장하는 장치
      * 트랙 (track)과 섹터 (sector)로 이루어져 있으며 sector를 기본으로 저장이 이루어짐
      * 비휘발성으로 데이터의 장기 저장에 사용되고 디스크 컨트롤러로 전반적인 동작 제어
    * Solid-state disks (SSD)
      * 비휘발성이며 마그네틱 디스크보다 빠른 반도체 저장장치
      * 내부의 플래시 메모리를 기반으로 하는 SSD가 사용됨



* Storage Hierarchy

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image04.PNG?raw=true" alt="image04" style="zoom: 67%;" />

  * 저장장치의 계층구조

  1. 레지스터 (register) : CPU 안에 있는 워드 크기의 메모리로 CPU가 명령을 실행할 때 레지스터 내의 메모리를 직접 접근함

  2. 캐시 (cache) : CPU가 가장 최근에 접근했던 명령이나 데이터의 복사본을 저장

  3. 메인 메모리 (main memory) : 현재 실행중인 프로그램을 저장하는 장소

  4. SSD와 마그네틱 디스크 : 보조 저장장치로 비휘발성 대용량 저장장치

  5. optical disk와 magnetic tapes : CD롬, 대용량 저장장치

     SSD 이하의 저장장치는 입출력 장치에 해당하므로 Device Driver라는 입출력 전용프로그램을 통해 접근

     

  * 컴퓨터 시스템의 저장장치들이 계층구조를 이루는 기준 : 속도, 가격, 휘발성
  * 속도 - 위쪽 저장장치일수록 저장된 데이터에 대한 접근속도가 빠르며 아래쪽일수록 저속장치
  * 가격 - 위쪽 저장장치일수록 단위 용량당 가격이 비싸고 아래쪽 장치일수록 저렴함
  * 휘발성 저장장치 :  레지스터, 캐시, 메인 메모리 (나머지는 비휘발성 장치)



* Caching
  * 컴퓨터 시스템에서 동작하는 컴퓨터 동작 성능 향상 기법
  * 주어진 하드웨어 하에서 최대한 빨리 데이터에 접근하기 위해서 Caching 사용
  * 저속의 저장장치에 저장된 데이터에 대해 일부의 복사본을 그보다 빠른 저장장치에 보관하여 성능 향상
  * Caching 기법에 따라 다르지만 보통 가장 최근에 사용한 데이터를 Caching하여 고속 저장장치에 보관함
  * 모든 데이터 접근 시에 고속 저장장치인 캐시에서부터 검색하고 캐시에 해당 데이터가 있을 시 그대로 접근하고 원하는 데이터가 없는 경우 원본이 있는 저속 저장장치에 가서 데이터를 접근함
  * 새로운 데이터를 접근하여 기존의 데이터를 갱신하는 것을 Cache replacement라고 함



* I/O Structure (입출력방식 대표적으로 2가지)
  * CPU가 입출력을 요청한 후에 해당 입출력이 완료될 때까지 기다리는 방식
    * 프로그램은 입출력을 요청하고 idle 상태에서 입출력 완료를 기다림
    * 한번에 하나의 입출력만 처리됨
  * CPU가 입출력을 요청한 후에 해당 입출력이 완료를 기다리지 않고 자신의 동작을 이어가는 방식
    * 나중에 입출력이 완료되어 interrupt를 받게 되면 그때 관련된 처리를 함
    * 동시에 여러개의 입출력이 병행될 수 있음
    * 운영체제는 내부적으로 Device-status table을 유지하며 각 입출력의 현재 상태를 관리



* Direct Memory Access Structure (DMA)
  * 주로 고속으로 동작하는 입출력 장치에서 사용하는 입출력 방식
  * 처음 입출력이 개시되는 과정만 CPU에서 이루어지며 이후 메모리에 있는 데이터는 CPU에 관여없이 해당 입출력 장치로 전송되어 입출력이 이루어짐
  * interrupt의 발생 또한 매 byte 단위가 아닌 일련의 block에 대한 입출력이 완료될 때 한번만 발생



* How a Modern Computer Works

<img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image05.PNG?raw=true" alt="image05" style="zoom:67%;" />

* 컴퓨터 시스템의 동작은 3가지 요소인 CPU, 메인 메모리, 입출력 장치로 이루어짐



* 컴퓨터 시스템 동작 과정 (폰 노이만 구조 컴퓨터, 내장 프로그램 구조 컴퓨터)
  1. 프로그램은 우선 메인 메모리에 저장되어 있어야함 (instructions and data)
  2. 메모리에 저장된 명령과 데이터는 버스를 통해 CPU로 이동되어 실행됨 (instruction execution cycle)
  3. CPU와 입출력 장치 간에는 데이터를 주고 받으며 입출력 동작이 이루어짐 (I/O request)
  4. 입출력 장치에 유형에 따라 메인 메모리와 직접 데이터를 주고 받는 DMA에 동작하기도 함



### 1.3 Computer System Architecture (컴퓨터 시스템 구조)

---

여러 프로세서들이 여러 메모리와 어떤 방식으로 연결되어 있는지 구성 형태를 나타내는 용어



* Single Processor System : 하나의 프로세서를 사용하는 시스템으로 대부분의 시스템이 이에 해당함

* Multiprocessor System : 두개 이상의 프로세서를 가지며 버스, 메모리와 주변 장치를 공유하는 시스템

  * 멀티 프로세서의 장점
    1. 처리량의 증가 - 프로세서의 개수가 여러개이므로 짧은 시간동안 더욱 많은 작업을 수행할 수 있음
    2. 경제성 - 싱글 프로세서 시스템보다 비용이 적게 듦
    3. 신뢰성의 증가 - 일부 프로세서가 고장나도 전체 컴퓨터 시스템의 동작을 이어나갈 수 있음
  * 현재 사용되는 형태 2가지
    1. Asymmetric Multiprocessing : 한 프로세서가 주처리기로 전체 시스템을 제어하고 다른 프로세서는 주처리기의 명령을 실행하거나 미리 지정된 태스크를 처리하는 형태
    2. Symmetric Multiprocessing : 각 프로세서가 운영체제와 사용자 프로그램을 동등하게 처리하는 형태
  * 프로세서의 메모리 접근 방식
    1. UMA : 모든 프로세서에서의 메모리 접근에 동일한 시간이 걸림
    2. NUMA : 일부 메모리는 다른 위치에서의 메모리보다 접근시간이 오래걸림



* A Dual-Core Design : 하나의 칩에 여러개의 코어를 포함시킨 시스템

  * 단일 코어를 갖는 여러개의 칩보다 효율적임 (칩 내에서 코어 간의 통신이 칩 간의 통신보다 빠르기 때문)

* Blade Server : 여러 개의 처리기 보드와 입출력 보드, 네트워크 보드들을 하나에 실시에 장착한 시스템

* Clustered System : 둘 이상의 독자적인 컴퓨터 시스템이 고속의 네트워크로 묶여 단일 시스템으로 동작

  * 랜이나 고속의 인터커넥트로 연결되어 있고 storage-area network (SAN)을 통해서 저장장치 공유
  * 고가용성 서비스 (high-availability) 를 제공하기 위해 사용됨
  * 고성능 컴퓨팅 환경 (high-performance computing - HPC) 을 제공하기 위해 사용됨



### 1.4 Operating System Structure

---

모든 운영체제는 Multiprogramming과 Timesharing 을 한다는 공통점이 있다.



* Multiprogramming

  * 운영체제는 한번에 여러 개의 job(실행해야하는 응용프로그램)을 메모리에 적재함

  * 운영체제는 메모리에 있는 job 중에서 하나를 선택해서 실행을 시작하고 실행된 job은 입출력을 요청하며 운영체제는 이 입출력이 완료되기를 기다리는 상태가 되는데 Multiprogramming 시스템에서는 단순하게 다른 job으로 전환을 해서 그 job을 실행함

    -> CPU를 효율적으로 사용하게 되고 매번 새로운 job을 동작하기 위해서 job  scheduler 함수가 동작함

* Timesharing (Multitasking, Multiprogramming의 확장 개념)

  * CPU가 다수의 작업들을 교대로 실행하지만 교대가 매우 빈번하게 이루어져 사용자는 각자 자기 프로그램이 실행되는 동안 상호작용을 할 수 있음
  * 사용자와 컴퓨터 시스템 간의 직접적인 통신을 하는 interactive computing 시스템을 제공함
  * 응답 시간(Response time)은 매우 짧아야 하며 전형적으로 1초이내가 됨
  * 동시에 많은 사용자가 컴퓨터를 공유하고 사용자는 적어도 하나의 프로그램을 (process) 실행시키게 됨
  * CPU scheduling: 동시에 여러개의 process가 실행가능한 상태인 경우에 이 중 하나를 선택하는 방법
  * swapping: 메모리에 유지해야하는 process가 너무 많은 경우에 일부 process를 디스크로 보냄
  * Virtual memory: process 전체가 메인 메모리에 있지 않아도 실행이 가능하게 해주는 메모리 관리 기법



### 1.5 Operating-System Operations (운영체제의 연산)

---

* Interrupt 구동 방식으로 동작

  * 소프트웨어 오류나 서비스 요청에 해당하는 exception이나 trap이 발생한다면 이를 처리하기 위해 운영체제의 일부인 exception 처리 루틴이 실행되며 운영체제가 동작함
  * 소프트웨어 interrupt
    * exception: CPU가 일반적인 명령을 수행하는 과정에서 일어나는 오류 때문에 발생 ex) 0으로 나눔
    * trap:  프로그램인 특수한 명령인 trap(CPU의 상태를 interrupt 발생 전으로 바꿈) 실행시켜 발생함

* Dual-mode: CPU 내 특정 레지스터의 bit (모드 비트) 값에 의해 결정됨

  * User mode (모드 비트의 값이 1)

    * 특권(privileged) 명령인 일부 명령을 실행할 수 없음
    * 보통 사용자 process가 실행됨

  * Kernel mode (모드 비트의 값이 0)

    * 특권(privileged) 명령인 일부 명령을 실행할 수 있음
    * 보통 운영체제 커널이 실행됨

  * Daul-mode 동작에 따른 사용자 process와 운영체제 커널간의 실행 전환 과정

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image06.PNG?raw=true" alt="image06" style="zoom: 67%;" />

    1. user process가 실행되는 동안은 모드 비트의 값이 1인 user mode 상태
    2. process는 실행 도중 운영체제의 특정 서비스가 필요해지면 trap 명령을 사용하여 system call 수행
    3. 발생된 trap을 처리하기 위해 커널이 실행되며 모드 비트가 0으로 바뀌며 kernel mode로 전환됨
    4. 사용자가 요청한 system call에 대한 처리가 이루어짐
    5. 사용자 process로 돌아가고 CPU의 모드 비트가 1로 바뀌며 user mode로 전환됨
    6. system call 호출 직후부터 실행을 이어나감



### 1.6 Process Management

---

* process: 실행중인 프로그램으로 시스템 입장에서 작업의 단위이고 보통 실행중인 능동적 단위 (active entity)
* process는 작업을 수행하기 위해서 하드웨어 자원이 필요함 (CPU, memory, I/O, files)
* process가 종료되면 사용한 자원이 회수됨
* 전형적으로 시스템에는 다중의 process가 존재하며 하나 또는 여러개의 CPU 상에서 동시에 실행됨
* Process Management Activities (운영체제의 역할)
  * process를 생성하고 삭제하며 실행을 잠시 멈췄다가 재개하는 역할을 함
  * process간 동기화 기능과 통신의 수단을 제공
  * process는 제한된 자원을 두고 경쟁할 때 부작용으로 교착상태가 발생하면 이를 해결하는 매커니즘 제공

 

### 1.7 Memory Management

---

* 모든 데이터는 처리 전후에 메인 메모리에 존재해야함
* 모든 명령은 실행되기 위해서 메인 메모리에 적재되어야 함
* 메모리 관리에 핵심은 언제 어떤 내용을 메모리에 저장할 것인가를 결정하는 일로 CPU 활용도를 높이고 사용자 응답 시간을 개선하는 방향으로 결정이 이루어져야 함
  * 메모리의 어떤 영역이 누구에 의해 사용되는지 결정
  * 어느 process가 메모리로 이동하고 메모리로부터 나와야하는지 결정
  * 메모리 공간의 할당과 해제 수행



### 1.8 Storage Management

---

* 운영체제는 정보의 저장에 대한 논리적이고 일관된 view를 제공함

  * 특정한 저장매체에 관계없이 보통 file이라는 논리적인 저장단위로 데이터를 저장할 수 있음

* File-System management

  * File 단위로 데이터를 저장할 수 있으며 Directory를 통해 File을 쉽게 분류함
  * 파일과 디렉토리 생성 삭제, 파일과 디렉토리 내용을 조작할 수 있는 수단
  * 파일 내용이 저장 장치에 특정 위치에 기록될 수 있도록 관리, 파일의 내용 삭제 방지 stable storage 제공

* Mass-Storage Management (대용량 저장장치 관리)

  * 디스크는 메인 메모리에 저장하기 어려운 데이터를 장기간 보존하는데 적절함
  * 컴퓨터의 전체성능은 disk subsystem과 algorithm의 성능에 크게 영향을 받음
  * 운영체제 기능: 빈공간 관리, 저장 장치 할당, 디스크 스케쥴링 (Disk scheduling)

* 컴퓨터 시스템에서 사용되는 각종 저장장치들의 성능

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap01/image07.PNG?raw=true" alt="image07" style="zoom:67%;" />

* I/O Subsystem

  * 운영체제 주요한 역할: 개별 하드웨어 장치의 특수성을 사용자로부터 숨기며 사용자 process 대신 입출력을 대신 수행해줌
  * 운영체제가 제공하는 입출력 기능
    * 입출력에 필요한 메모리 공간 관리 - buffering, caching, spooling
      * buffering: 입출력 중인 데이터를 저장장치에 임시로 저장하는 방법
      * caching: 저속의 저장장치의 데이터 중 일부를 고속 저장장치에 복사시키는 방법
      * spooling: 입출력 데이터를 장치에 보내는 대신에 메모리공간에 저장하고 나중에 처리하는 방법
    * 운영체제는 특정 입출력 장치를 읽고 쓰는 기능을 가진 프로그램인 Device deriver를 제공함
    * 범용 device-driver interface를 제공하여 process는 특정 하드웨어에 상관없이 일관된 방법으로 입출력 장치를 사용할 수 있음 



### 1.9 Protection and Security

---

* Protection(보호): 어느 사용자 또는 process가 어떤 자원에 접근할 수 있는지 제어하는 기능
* Security(보안): 시스템을 외부 또는 내부의 공격으로부터 방어하는 기능 (바이러스, worms, dos 공격 등)
* 시스템에서는 사용자들을 구분하고 User id를 부여하여 각 사용자가 어떤 일을 할 수 있는지 정의
* 특정조건이 맞는 경우 사용자에 임시로 권한을 올려주는 Privilege escalation이 이루어지기도 함



### 1.11 Computing Environments

---

* Traditional
  * 전통적인 컴퓨팅 환경은 다수의 독립적인 범용 컴퓨터와 제한적인 네트워크 환경으로 이루어져 있음
  * 하지만 인터넷의 발달로 거의 모든 시스템이 연결되어 현재는 원격 접근과 이동성을 다양한 형태로 제공
  * Network computers (thin clients)는 보다 높은 수준의 보안이나 쉬운 유지 보수가 필요한 경우에 전통적인 핏이나 work station을 대체하고 있음
* Mobile (Android, iOS)
  * 스마트폰이나 태블릿 등 장치가 가볍고 이동가능한 컴퓨터로 휴대용 접근성을 얻음
  * GPS나 gyroscope 등의 독특한 기능을 사용하고 용용함
* Distributed (분산시스템)
  * 물리적으로 떨어진 컴퓨터 시스템 집합으로 사용자가 다양한 자원을 접근할 수 있도록 네트워크로 연결됨
  * 네트워크는 컴퓨터 시스템 간의 통신 경로로 TCP/IP가 일반적 네트워크 프로토콜임
  * 네트워크 운영체제는 네트워크를 통한 파일의 공유, 다른 컴퓨터 상의 process 간 메시지 교환을 제공
* Client-Server
  * PC의 성능 향상과 저렴해진 가격으로 기존의 중앙집중식 시스템에 연결된 터미널이 PC로 대체됨
  * 오늘날의 시스템은 Client 시스템에 의해서 만들어진 요청을 처리하기 위한 서버 시스템으로 동작
  * 서버의 종류
    * 계산서버: client가 작업을 요청할 수 있는 인터페이스를 제공하고 요청을 처리해서 client에 돌려줌
    * 파일서버: client가 파일을 생성, 갱신, 읽기 및 제거할 수 있는 파일 시스템 interface 제공
* Peer-to-Peer
  * P2P는 client와 server가 서로 구별되지 않으며 시스템 상의 모든 노드가 peer로 간주되고 각 peer는 서비스를 요청하느냐 제공하느냐에 따라 client나 server 또는 둘 다로 작동함
  * P2P 시스템에 참여하려면 노드는 peer 간 네트워크에 참여해야함
    * 네트워크에 참여하는 노드가 네트워크에 잘 알려진 검색 서비스에 자신의 서비스를 등록하는 경우
    * 특정 discovery protocol을 통해 기존 peer 네트워크에 참여하는 경우
* Virtualization
  * 운영체제가 다른 운영체제 내에서 하나의 응용프로그램으로 실행될 수 있게 함
  * Emulation은 가상화의 한 기법으로 소스 CPU의 타입이 타깃과 다른 경우 사용
    * 컴퓨터 언어가 기계어가 아닌 고급 수준의 형태로 번역된 경우 - Interpretation
  * Virtualization은 특정 CPU를 위해서 컴파일된 운영체제를 같은 CPU 다른 OS에서 실행할 수 있게 함
* Cloud Computing
  * 계산, 저장장치와 응용을 네트워크를 통한 서비스로 제공하는 방식
  * 가상화를 기반으로 다양한 형태가 존재함
    * Public cloud - 누구나 사용가능한 cloud, Private cloud - 한 회사 내에서 사용가능한 cloud
    * Hybrid cloud - 공공과 사설 서비스를 모두 갖고 있는 cloud
    * SaaS - 인터넷을 통해 사용가능한 응용, PaaS - 인터넷을 통해 제공되는 소프트웨어 스택
    * IaaS - 인터넷을 통해 사용가능한 서버나 저장장치
* Real-Time Embedded Systems
  * 실시간 임베디드 시스템은 최근 가장 많이 사용되는 시스템으로 자동차 엔진, 공장의 로봇, 각종 가전제품 등에서 찾아볼 수 있음
  * 특정한 작업만을 사용하고 제한된 기능을 제공