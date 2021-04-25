# chapter 2 : Operating System Structures

2021.03.10



1. 운영체제가 갖고 있는 수많은 기능들을 사용자에게 제공하는 서비스에 대해 알아봄
2. 운영체제의 사용자에게 시스템을 접근할 수 있는 수단인 사용자 인터페이스 소개
3. 응용프로그램이 운영체제의 서비스를 이용할 수 있는 수단인 System Call 소개
4. 핵심 기능을 수행하는 커널 외의 포함된 다양한 시스템 프로그램 소개
5. 운영체제의 설계와 구현에 대한 다양한 원칙을 다루고 운영체제의 구조와 장단점을 비교
6. 컴퓨터 시스템이 시작돼서 운영체제가 동작하는 과정인 부팅 과정을 알아봄



### 2.1 Operating System Services

---

* 운영체제는 응용 프로그램이 실행할 수 있는 환경을 제공하고 응용 프로그램과 사용자에게 각종 서비스 제공

* 운영체제가 제공하는 서비스

  * 사용자 인터페이스: Command-Line (CLI), Graphics User Interface (GUI), Batch 등
  * 프로그램 실행: 디스크 등의 보조기억장치에 저장되어 있는 프로그램을 메인메모리로 읽어 실행함
  * 입출력 연산: 응용 프로그램이 필요로 하는 모든 입출력을 대행하여 수행
  * 파일 시스템 조작: 응용 프로그램이 파일과 디렉토리를 읽고 쓸 수 있도록 해주고 파일의 생성과 삭제, 검색, 파일 정보의 리스트, 접근권한 관리 등의 서비스를 제공

* Operating System Services (Cont.)

  * 운영체제가 개별 프로그램을 위해 제공하는 기능
    * 통신 기능(Communications): 운영체제는 프로세스간의 통신 기능을 제공
      * 공유 메모리 사용과 메시지 전달로 컴퓨터 내에서 프로세스간 통신이 이루어짐
    * 오류 탐지 기능(Error detection): 운영체제는 컴퓨터 사용시 발생하는 오류를 지속적으로 모니터링함
      * CPU나 메인 메모리의 오동작 또는 응용 프로그램의 논리적 오류 발생시 컴퓨터 시스템이 동작을 이어 나갈 수 있게 조치를 취하고 이처럼 오류 발생 원인을 파악하는 과정을 디버깅이라고 함
  * 운영체제가 시스템 전체의 효율적 사용을 위해서 수행하는 기능
    * 자원 할당 기능(Resource allocation): 하드웨어 자원 (CPU, 메인 메모리, I/O device), 소프트웨어 자원 (파일, lock)을 실행중인 프로그램인 process에게 이를 할당하는 역할을 함
    * Accounting: 사용자가 사용한 자원을 기록하는 기능 
    * 정보 보존과 보안(Protection and Security)
      * Protection: 모든 시스템 자원에 대한 접근을 적절히 통제하는 기능
      * Security: 사용자 인증, 불법적인 접근 차단으로 컴퓨터 시스템을 보호하는 기능

* A View of Operating System Services

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image01.PNG?raw=true" alt="image01" style="zoom:67%;" />

  1. 그림의 최상단에는 사용자 프로그램이 있고 가장 아래쪽에는 하드웨어가 있으며 사용자 프로그램은 실행을 위해서 하드웨어 자원이 필요함
  2. 사용자 프로그램 바로 아래에는 사용자가 컴퓨터 시스템을 쉽게 사용할 수 있도록 해주는 사용자 인터페이스인 GUI나 CLI가 있음 (특수 목적의 사용자 프로그램)
  3. System call은 응용 프로그램이 하단의 운영체제 기능을 호출하는데 사용하는 프로그래밍 인터페이스
  4. 운영체제 커널은 하드웨어 바로 위에서 동작하며 프로그램의 실행, 입출력 등의 운영체제 서비스 제공



### 2.2 User Operating System Interface

---

* CLI (command interpreter): 스크린과 키보드 장치를 기반으로 하는 사용자 인터페이스
  * 사용자가 키보드 입력을 통해 명령을 입력하면서 프로그램을 실행함
  * 명령이나 프로그램 실행 과정에서 출력 등은 모두 화면을 통해 display 됨
  * 커널 내에 구현되거나 별도의 응용 프로그램으로 제공됨
  * 응용 프로그램으로 여러 종류가 제공되는 interpreter를 shells 이라고 함
  * 사용자로부터 명령을(자체적 처리하는 내장 명령, 다른 프로그램의 명령) 받고 이를 해석해서 실행함
* GUI (Graphics User Interface)
  * 사용자가 직접 명령을 입력하지 않고 desktop이라는 윈도우 시스템을 사용하는 인터페이스
  * 마우스를 사용하여 프로그램, 파일, 시스템 기능들을 나타내는 화면 이미지를 통해 실행하고 조작
* 사용자 개인의 선호에 따라 사용자 인터페이스를 CLI와 GUI 중에 선택하여 사용함



### 2.3 System Calls

---

* System Calls

  * 운영체제가 제공하는 서비스를 사용할 수 있도록 하는 프로그램 인터페이스
  * 일반적으로 고급언어인 C나 C++ 의 함수 형태로 제공됨
  * 응용프로그램은 원하는 System Call을 호출하여 커널의 서비스를 제공 받을 수 있음

* System Calls (Cont’d)

  * 실제 응용 프로그램 개발자는 System Call을 직접 호출하기 보다 Application Program Interface (API) 를 사용하여 프로그램을 작성하는 경우가 많음
  * API는 System call보다 높은 수준의 인터페이스로 같은 API를 지원하는 다른 시스템에서도 해당 프로그램이 잘 컴파일되고 실행되므로 사용하기 더 편리함
  * 많이 사용되는 API: Win32 API for Windows, Java API for the Java virtual machine (JVM)

* Example of Standard API

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image02.PNG?raw=true" alt="image02" style="zoom:67%;" />

  * open된 파일에서 데이터를 읽어들이는 기능을 하는 read 함수, 대부분의 Unix에서 사용 가능함
  * fd라는 오픈된 파일에서 count 바이트 만큼 읽어서 이를 buf 포인트가 가르키는 곳으로 복사하는 기능
  * 실제 읽기 기능을 수행한 뒤 읽어들인 byte 수를 반환하며 함수가 종료됨



* System Call Implementation (System Call이 호출되고 처리된 후 복귀하는 과정)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image03.PNG?raw=true" alt="image03" style="zoom:80%;" />

  * 가장 위쪽의 운영 프로그램은 유저 모드에서 동작하고 아래쪽의 커널 프로그램은 커널에서 동작하며 그 사이에는 system call interface가 있음

  * 커널은 제공하는 system call 마다 번호를 부여하여 내부에 번호별로 해당 system call을 처리하는 포인터들로 이루어진 table을 갖고 있으며 커널 코드 안에는 system call을 처리할 수 있는 함수가 있음
  * 동작 과정
    1. 응용 프로그램이 동작하다가 system call을 호출 (예시에서 open())
    2. 호출로 system call interface를 거쳐서 커널 코드가 시작되게 되며 이 과정에서 커널 모드로 바뀜
       * 커널에서는 가장 먼저 해당 system call 번호를 통해 처리 함수의 시작 주소를 찾음 (예시에서 i)
    3. 커널 내에서 처리 함수로 제어가 옮겨져서 처리 함수가 시작되는 과정
    4. 처리 함수가 수행을 마치고 종료를 하면 복귀가 이루어짐 (system call의 복귀)
    5. 실행 모드가 유저 모드로 바뀌고 원래 응용 프로그램의 system call 호출 직후로 복귀가 완료



* System Call Parameter Passing

  * system call 마다 미리 정의된 파라미터가 있을 수 있음

  * system call을 호출하는 과정에서는 유형을 나타내는 번호와 해당 call의 파라미터가 전달되어야 함

  * system call parameter을 전달하는 방법 (3가지)

    1. 파라미터를 CPU 레지스터에 담아 전달하는 방법

       * 파라미터가 많지 않고 크기가 작은 경우 사용

    2. 파라미터를 메모리 영역에 저장한 후 그 영역의 시작 주소를 레지스터에 담아 파라미터로 전달

       <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image04.PNG?raw=true" alt="image04" style="zoom:80%;" />

       * 예시에서 13은 system call의 고유 번호이며 x는 파라미터가 담긴 사용자 프로그램 공간의 주소
       * 이를 전달받은 커널은 고유 번호를 통해 해당 system call을 확인할 수 있고 사전에 약속된대로 13번 system call parameter는 레지스터의 담긴 주소를 접근하여 얻을 수 있음

       * 파라미터의 개수와 크기에 제약이 없음

    3. 프로그램 스택을 사용하는 방법

       * system call parameter를 스택에 push하여 전달하면 커널이 pop하여 쓸 수 있음
       * 파라미터의 개수와 크기에 제약이 없음

* Standard C Library Example (표준 C 라이브러리 함수와 system call 간의 관계)

  * C 프로그램 개발 환경이 갖추어진 모든 시스템은 표준 C 라이브러리를 제공함

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image05.PNG?raw=true" alt="image05" style="zoom:80%;" />

  1. 응용 프로그램이 printf를 호출하면 C라이브러리는 이 호출을 가로채 운영체제에 필요한 system call을 함
  2. 이 경우에는 printf 함수의 화면 출력 기능을 구현하기 위해 write라는 system call이 사용됨
  3. 커널에서 write라는 system call이 마무리되고 return하면 write의 반환값을 C 라이브러리가 받음 
  4. C 라이브러리는 이 반환값을 적절한 printf의 반환값으로 바꾸어 운영 프로그램에 돌려줌



### 2.4 Types of System Calls

---

* System Call의 종류 (6가지)

  * Process control
    * end(process는 수행을 마치고 종료해야함), abort
    * create process(새로운 프로그램을 실행할 때 사용), load, execute
    * get process attributes, set process attributes (process 속성을 얻고 변경할 수 있음)
    * wait for time (system call을 통해 실행을 지연시킬 수 있음)
    * wait event, signal event (기다리는 사건이 발생이 일어나면 해당 신호를 보냄)
    * allocate and free memory (메모리 할당을 요청하고 사용이 끝난 메모리 반납)
    * memory Dump: process 중 오류가 발생했을때 process의 메모리 상 이미지 파일
    * Debugger: 응용 프로그램의 오류를 찾는 디버깅 소프트웨어
    * Locks: process가 공유 데이터를 잠그고 푸는 system call
  * 파일 조작, 장치 조작, 정보의 유지 보수, 통신, 보호

* Example: MS-DOS (단일 태스킹 시스템)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image06.PNG?raw=true" alt="image06" style="zoom:80%;" />

  * (a) 시스템이 시작된 직후: 메모리 상에 커널과 명령 해석기만 있음
  * (b) 프로그램이 실행될 때: 가능한 많은 메모리를 program에게 제공하기 위해 명령 해석기의 일부를 실행하는 program으로 덮어쓰게 됨
  * Single-tasking(단일 태스킹 방식)으로 하나의 프로그램을 실행하기 위해 간단한 방법을 사용하며 새로운 process를 추가로 생성하지 않음
  * 명령 포인터를 프로그램의 첫번째 명령으로 설정하여 프로그램의 실행을 게시함
  * 프로그램 실행중 종료나 오류가 발생하면 trap이 발생하고 명령 해석기 남은 부분이 실행됨
  * 디스크로부터 덮어쓴 부분을 다시 적재하고 오류 코드를 사용자가 이용할 수 있게 해줌

* Example: FreeBSD (다중 태스킹 시스템)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image07.PNG?raw=true" alt="image07" style="zoom:80%;" />

  * 사용자가 시스템에 로그인하면 미리 지정해놓은 shell이 수행됨
  * 사용자로부터 명령을 받아 해당하는 프로그램을 실행하는 점에서  MS-DOS와 유사함
  * 그러나 다중 태스킹 시스템이므로 명령 해석기는 다른 프로그래밍 실행되는 중에도 실행을 계속할 수 있으며 shell과 명령에 해당하는 process가 동시에 존재함
  * shell은 fork()라는 시스템콜로 새로운 process를 생성하고 생성된 process는 exec()이라는 시스템콜로 새로운 프로그램이 실행될 수 있도록 함
  * shell에 명령이 내려지는 방식에 따라 process가 종료하기를 기다리지 않고 자신의 실행을 이어나갈 수 있으며 다른 프로그램의 실행을 요청하거나 관리할 수 있음
  * process가 끝나면 종료하기 위해 exit 시스템 콜을 사용하며 이를 통해 적절한 상태 코드가 명령 해석기로 전달됨

* Types of System Calls

  * File management
    * 파일을 삭제하고 생성하는 시스템 콜을 제공함
    * 파일을 열고 닫고 읽고 쓰고 접근위치를 바꾸는 기능도 시스템 콜에 있음
    * 파일의 속성을 얻거나 변경하는 시스템 콜도 존재함
  * Device management 
    * process가 동작하는 과정에서 추가의 자원이 필요할 때 이를 요청하는 시스템 콜
    * process가 자원을 사용하고 반납하는 과정이 시스템 콜로 이루어짐

* Types of System Calls (Cont.)

  * Information maintenance (정보 유지)
    * 대부분의 시스템은 현재의 시간과 날짜를 되돌려주는 시스템 콜을 갖고 있음
    * time profile: 프로그램이 특정 메모리 위치에서 실행한 시간의 양
    * timer interrupt가 빈번히 발생할 때 프로그램의 각 분야에서 소비한 시간을 알 수 있음
  * Communications (통신)
    * 메시지 전달 방식
      * 통신하는 두 process가 메시지를 통해 정보를 주고 받음
      * 정보를 직접 교환하거나 메일 박스를 통해 간접적으로 교환함
      * 두 process 간의 system call에 의해 연결이 이루어져야 함
    * 공유 메모리 방식
      * 한 process가 다른 process가 소유한 메모리 영역에 접근해야 하며 이를 위해 공유 메모리를 생성하고 공유 메모리를 자신의 일부로 포함시키는 시스템 콜이 필요함
  * Protection (보호)
    * 운영체제가 제공하는 자원에 대한 접근을 제어하는 것을 말함
    * 보호와 관련된 시스템 콜은 권한을 얻거나 설정하는 시스템 콜로 이루어짐

* Examples of Windows and Unix System Calls

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image08.PNG?raw=true" alt="image08" style="zoom:80%;" />

  * Windows와 Unix의 시스템 콜
    * Process Control
      * fork(): process를 새로 생성하는 시스템 콜
      * exit(): process를 종료하는 시스템 콜, process의 종료값을 남길 수 있음
      * wait(): 다른 process의 종료를 기다리는 시스템 콜
    * File Manipulation
      * open(): 파일 사용을 시작하기 위해 파일을 개방하는 시스템 콜
      * read(): 오픈된 파일에서 데이터를 읽는 시스템 콜
      * write(): 오픈된 파일에 데이터를 쓰는 시스템 콜
      * close(): 파일 사용을 마무리하는 시스템 콜
    * Device Manipulation
      * ioctl(): 장치의 속성을 읽거나 설정하는 범용 시스템 콜
      * read(), write()
    * Information Maintenance
      * getpid(): process의 id를 얻는 시스템 콜
      * alarm(): 지정한 시간이 되면 signal이 발생하는 시스템 콜
      * sleep(): process를 지정한 시간만큼 수행 정지 시켰다가 깨어나게 하는 시스템 콜
    * Communication
      * pipe(): 두 개의 process간 단방향 통신 채널을 만드는 시스템 콜
      * shmget(): 공유 메모리를 생성하는 시스템 콜
      * mmap(): 생성된 공유 메모리를 자신의 주소 공간에 붙이는 시스템 콜
    * Protection
      * chmod(): 파일마다 정해져있는 접근권한을 변경하는 시스템 콜
      * umask(): 새로운 파일에 대해 기본적으로 적용되는 접근 권한을 정하는 시스템 콜
      * chown(): 파일의 소유자를 설정하는 시스템 콜



### 2.5 System Programs

---

* System Programs
  * 사용자나 개발자가 프로그램을 개발하고 실행하는데 편리한 환경을 제공하는 소프트웨어
  * 운영체제를 설치하는 과정에서 기본적으로 설치되거나 별도인 패키지로 설치함
* System Programs의 종류
  * File management: 윈도우 운영체제의 탐색기 프로그램과 비슷한 역할
    * 파일을 생성하고 삭제하고 인쇄하는 등 파일의 조작을 도와줌
  * Status information: 시스템의 상태 정보를 사용자에게 전달하는 역할
    * 시스템의 상태 뿐만 아니라 성능과 디버깅 등 시스템의 중요한 정보도 전달함
    * windows에서는 환경설정 정보를 저장하고 검색하는 registry 편집기도 제공함
  * File modification: 텍스트 편집기와 같이 파일을 생성하고 수정하는 기능
    * 파일의 내용을 검색하거나 형식을 변환하는 시스템 프로그램들도 사용됨
  * Programming-language support: 컴파일러, 어셈블러, 인터프리터 등
  * Program loading and execution: 프로그램을 메모리에 load하고 실행하는 프로그램
  * Communications: process나 사용자 간의 접속을 제공하는 위한 프로그램
* System Programs의 기능에 따른 종류
  * Background Services: 시스템의 시작과 함께 늘 동작하는 프로그램
    * services, subsystems, daemons라고도 부름
  * Application Programs: 일반적인 문제를 해결하는 응용 프로그램
    * 워드 프로세서, 스프레드 시트, 게임 등이 이에 해당함



### 2.6 Operating System Design and Implementation

---

* 시스템을 설계하는데 발생하는 문제
  * 시스템 goal과 specification을 정의하는 문제
    * 어떤 하드웨어를 사용하고 어떤 종류의 시스템인지에 따라 다름
  * User goals과 System goals
    * User goals: 시스템의 사용자 입장에서 시스템에 기대하는 특징
      * 운영체제가 사용하기 쉽고 편리하며 배우기 쉽고 안전하고 신속함
    * System goals: 운영체제가 설계구현, 유지보수가 쉬어야함
      * 적응성, 신뢰성, 효율성을 갖고 있으며 오류없이 동작해야함
    * 운영체제 설계에서의 원칙: mechanism 으로부터 policy를 분리
      * policy: 정책으로 무엇을 할것인지를 결정하는 것
      * mechanism: 기법으로 일을 어떻게 할것인지를 결정하는 것
      * 시스템의 유연성을 부여하는데 매우 중요함
* Implementation (운영체제의 구현)
  * 다양한 방법
    * 초창기는 assembly 언어로 구현했지만 요즘은 대부분 고급언어로 구현함
  * 실제 운영체제의 구현에서는 다양한 언어들이 함께 사용됨
  * 운영체제를 고급언어로 구현할수록 다른 하드웨어로 이식(Port)할 때 작업이 쉬움
  * Emulation 소프트웨어로 운영체제를 테스트하고 개발함



### 2.7 Operating System Structure

---

* 운영체제는 잘 동작하고 쉽게 변경할 수 있도록 구조를 만드는 것이 중요함

* 운영체제의 간단한 구조 (MS-DOS)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image09.PNG?raw=true" alt="image09" style="zoom:80%;" />

  * MS-DOS device drivers와 resident system program이 MS-DOS의 커널
  * 커널 대부분의 기능은  PC의 입출력 접근 프로그램인 ROM BIOS에 의존하고 있음
  * 커널의 일부 기능은 RAM에 상주하는 resident system program이 담당

* UNIX의 구조

  * UNIX의 운영체제는 시스템 프로그램과 제한적인 구조를 갖고 있는 커널로 이루어짐
  * Systems programs: 사용자 명령을 처리하는 CLI에 해당하는 셀과 프로그램 개발을 위한 컴파일러, 인터프리터, 시스템 라이브러리로 구성됨
  * 커널: 위에는 응용 프로그램으로부터 시스템 콜을 받을 수 있는 시스템 콜 인터페이스와 아래에는 하드웨어 인터페이스가 있으며 그 사이에는 운영체제의 모든 기능이 담겨있음

* Layered Approach (계층 구조)

  * 전체 시스템을 여러 개의 layer로 나눠서 설계됨
  * 커널이 여러 개의 계층으로 나뉘는 경우 각 계층은 데이터와 이를 조작하는 연산으로 구성
  * 구현과 디버깅이 간단하다는 장점이 있음
  * 하나의 동작이 이루어지는데 여러 계층을 거쳐야해서 효율성이 떨어진다는 단점이 있음

* Microkernel System Structure

  * 기존의 microkernel의 일부 구성요소를 커널로부터 떼어내어 사용자 수준 프로그램으로 구현하고 커널 안에 기본적인 기능을 남겨 구현한 구조이며 대표적인 예로 Mach가 있음
  * Client program과 Service program 간의 통신 기능을 제공 (메시지 전달 방식 사용)
  * 장점: 운영체제의 확장이 쉽고 다른 하드웨어와의 이식이 쉽고 보안성과 신뢰성이 높아짐

* Modules (loadable kernel modules)

  * 커널의 핵심적인 기능 이외에 동작중에 추가될 수 있는 서비스를 구현한 형태
  * 특정한 기능을 커널에 추가하지 않고 커널 모듈로 구현하면 해당 기능을 수정하거나 추가하기 위해서 커널 모듈만 컴파일하면 되어 편리하고 커널을 작은 규모로 유지할 수 있음

* Hybrid Systems

  * 대부분의 운영체제는 여러 방식을 혼용하는 Hybrid Systems을 기반으로 함
  * Windows 또한 monolithic 구조를 기반으로 microkernel을 포함하고 있음
  * Mac OS 또한 계층 구조와 Cocoa, Mach를 기반으로 동작하는 Hybrid Systems임

* iOS

  * Apple의 모바일 OS로  iPhone , iPad에 사용됨
  * Cocoa Touch, Media service, Core services, Core operating system을 기반으로 계층 구조로 이루어져 있음

* Android

  * 안드로이드 스마트폰과 태블릿에 사용됨
  * 오픈소스로 다양한 플랫폼에서 사용되고 Linux를 기반으로 만들어짐
  * Runtime environment로 core library와 함께 Dalvik이라는 Java virtual machine이 포함



### 2.10 System Boot

---

* 컴퓨터 시스템에 전원이 들어오면 CPU는 메모리 상의 특정 주소로부터 명령을 갖고와서 실행함
* 운영체제는 하드웨어가 자신을 접근하여 시작할 수 있도록 하는 방식에는 2가지가 있음
  * Rom 영역의 bootstrap loader 프로그램이 운영체제 커널의 위치를 찾아서 메모리에 로드하고 시작시킴 (매번 동일한 위치의 동일한 운영체제만 부팅시킬 수 있어 유연성이 떨어짐)
  * Rom의 코드가 디스크 등의 저장 장치의 boot block을 읽어서 실행시키고 이 boot block에서 운영체제를 로드함 (일반적으로 많이 사용됨)
* GRUB를 사용하면 다양한 disk patition으로부터 커널을 쉽게 선택하여 부팅할 수 있음



### Separating Mechanism and Policy

---

* Everyday example of mechanism/policy separation (실생활의 예 - 카드키)

  * mechanism: 문을 여는 방법 (카드 접촉)
  * policy: 출입자 구분 (데이터베이스를 통해 수정)
  * mechanism과 policy가 분리됨

* Separating mechanism from policy - function implementation (mechanism과 policy 구분)

  * 사각형 면적을 구하는 함수 예시

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image10.PNG?raw=true" alt="image10" style="zoom:80%;" />

* Adding some exception/error handling (오류와 예외 수정)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image11.PNG?raw=true" alt="image11" style="zoom: 67%;" />

* rect_area() vs. rect_area2()

  * mechanism: 사각형의 면적을 구하는 것
  * policy: 부적절한 parameter 오류 처리
  * 오류 상황과 중복에 있어서 두 함수가 다르며 mechanism과 policy가 구분이 안되어 문제

* Example should be implemented like this (mechanism과 policy가 구분됨)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap02/image12.PNG?raw=true" alt="image12" style="zoom:67%;" />