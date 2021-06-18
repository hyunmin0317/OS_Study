# chapter 7 : Main Memory

2021.05.06

<br>

## 7.1 Background

* Background
  * 프로그램은 실행을 하기 위해서 디스크로부터 메인 메모리에 로드되어야 함
  * 프로그램은 디스크에서 실행파일의 형태로 존재하다가 사용자가 프로그램의 실행을 요청하면 디스크로부터 실행파일이 읽혀져서 해당 프로세스의 메모리 영역으로 로드됨
  * 메인 메모리와 CPU 레지스터는 CPU가 직접 접근할 수 있는 유일한 저장장치
  * 레지스터는 CPU 내부의 저장장치이고 메인 메모리는 CPU와 구분되는 별도의 저장장치
  * 메인 메모리는 각각 주소가 붙어있는 바이트의 집합으로 CPU로부터 요청을 받음
  * 레지스터는 CPU 내의 저장장치로 CPU 입장에서 매우 빨리 접근할 수 있음 (1 CPU clock cycle 이내의 접근가능)
  * 메인 메모리의 접근에는 수 clock cycle이 소요될 수 있고 그동안 CPU는 stall 될 수 있음
  * 메인 메모리 접근의 한계를 극복하기 위해서 메인 메모리와 레지스터 중간에 Cache를 사용함
  * CPU의 현재 상태에 따라 허가된 메모리 영역만 접근할 수 있도록 하는 protection이 필요함

<br>

* Base and Limit Registers (protection에 대한 내용)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image01.PNG?raw=true" alt="image" style="zoom: 67%;" />

  * CPU가 메모리를 접근할 때에는 현재 허용된 범위 내에서 접근하는 것이 중요함
  * protection을 보장할 수 있도록 CPU에는 base와 limit 레지스터가 있으며 현재 접근할 수 있는 메모리 영역을 접근하는 역할을 함
    * base 레지스터: 접근가능한 영역의 시작 주소를 가리키고 있음
    * limit 레지스터: base 레지스터로부터 시작해서 접근가능한 영역의 마지막 범위인 끝을 가리키고 있음
  * CPU는 메모리 접근시 접근되는 주소가 두 레지스터가 정의하는 범위 내에 있는지 항상 검사함

<br>

* Hardware Address Protection (protection이 이루어지는 과정)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image02.PNG?raw=true" alt="image" style="zoom: 67%;" />

  1. CPU가 메모리를 접근할때마다 주소가 만들어짐 
  2. 값이 base 레지스터보다 큰지 그리고 base+limit 레지스터보다 작은지 확인함
     * 값이 base+limit보다 큰 경우 trap이 발생함
  3. 메모리에 대한 접근이 이루어짐

* Address Binding

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image03.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Address Binding: 프로그램이 내부적으로 사용하는 주소
  * 프로그래밍 개발 단계의 과정
    1. 소스코드를 컴파일하여 오브젝트 모듈을 만듦
    2. 오브젝트 모듈을 링커가 최종적으로 실행가능한 형태의 실행 모듈로 만들게 됨
    3. 실행모듈은 로더를 거쳐서 최종적으로 메모리에 로드됨
  * binding이 이루어지는 경우
    * Compile time: 컴파일을 해서 만들어지는 오브젝트 파일 내에서 만들어지는 주소가 최종 실행의 주소와 같은 경우
      * 프로그래머가 프로그램 내에서 최종 물리 주소를 결정하고 이를 사용하여 프로그래밍 한 경우
      * 항상 동일한 실행 위치에서만 실행해야함
    * Load time: 컴파일 하는 과정에서는 최종 주소가 확정되지 않고 상대적인 주소를 사용하는 경우
      * 매번 실행이 될때마다 다른 물리 주소에서 실행이 시작될 수 있음 
    * Execution time: 최종 주소에 대한 결정이 실행을 시작하는 순간에 결정되는 경우
      * 한번 결정된 주소가 실행중에 바뀔 수 있음
      * 프로세스가 실행 중에 메모리의 한 위치에서 다른 위치로 이동할 수 있음

<br>

* Logical vs. Physical Address Space
  * logical address: CPU가 메모리에 접근할 때 만들어내는 주소 (virtual address)
  * physical address: 메모리가 동작하면서 다루는 주소
  * 컴파일 타임 시에 binding을 하는 시스템 또는 로드 타임에 binding을 하는 시스템에서는 논리 주소와 물리 주소가 동일함
  * execution-time binding 하는 시스템에서는 논리 주소와 물리 주소가 다르며 메모리 접근시에 주소의 변환이 이루어져야함
  * Logical address space: 실행하는 프로그램이 만들어내는 모든 논리 주소
  *  Physical address space: 실행중에 실제 메모리에 접근되는 순간의 최종 메모리 상의 주소가 갖는 값들의 공간

<br>

* Memory-Management Unit (MMU)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image04.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * CPU가 만들어내는 논리 주소를(가상 주소) 물리 주소로 변환하여 mapping하는 CPU 내의 하드웨어 장치
  * 다양한 방식들이 있으며 가장 간단한 변환방식은 relocation 레지스터를 사용하는 방식
    * 프로그램의 모든 논리 주소에 대해 MMU가 매번 relocation 레지스터 값을 더해서 최종 물리주소를 만들어내는 방식
    * physical address = logical address + relocation register
  * 응용 프로그램 입장에서는 프로그램 내에서 주소를 다룰 때 논리 주소만을 다룸

<br>

* Dynamic Linking and Shared Library
  * Static linking: 시스템의 라이브러리와 프로그램의 코드가 linker에 의해 합쳐져서 하나의 실행파일이 만들어지는 과정
  * Dynamic linking: 개별 프로그램에 라이브러리를 포함시키지 않고 linking을 실행시 까지 뒤로 미루는 과정
  * Dynamic linking에 라이브러리 함수가 프로그램에 포함되는 대신에 stub이라는 작은 코드가 실행파일에 포함됨
  * 운영체제는 프로그램을 실행할때 동적 라이브러리가 메모리에 있는지 확인하고 없다면 메모리에 추가함
  * Dynamic linking은 여러 프로그램들이 자주 사용하는 라이브러리 함수에 적용하면 효과적
  * Dynamic libraries를 shared libraries라고도 함
  * 동일한 라이브러리라도 여러 버전이 있을 수 있음

<br>

## 7.2 Swapping

* Swapping

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image05.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * 메인 메모리에 적재되어 있는 프로세스의 개수 (degree of multiprogramming)가 많은 경우에 일부 프로세스를 임시로 디스크에 내보냈다가 메인 메모리 공간에 여유가 생겼을 때 다시 불러오는 기법
  * 전체 프로세스의 크기가 컴퓨터 시스템의 물리 메모리의 크기보다 클 수 있음
  * Backing store: 하드디스크는 큰 저장공간을 갖고 있고 읽고 쓰는데 비교적 속도가 빠르기 때문에 swapping 용 저장장치로 디스크가 사용됨
  * swapping을 하는데 걸리는 시간은 디스크를 읽고 쓰는 transfer time이 대부분의 시간을 차지함

<br>

* Context Switch Time including Swapping
  * Dispatcher가 다음에 실행할 프로세스를 선택했지만 프로세스가 swap out되어 디스크에 머무르는 프로세스이며 디스크의 빈공간이 없는 경우 현재 메모리에 있는 프로세스 중에 하나를 내보내고 원하는 프로세스를 불러드려야함 (시간이 오래걸림)
  * 사용자 프로세스의 크기를 100MB라고 가정하고 디스크 전송률이 50MB/s 일때
    * swap out 시 2000ms, 프로세스를 불러 드리는데 2000ms가 걸림
    * 전체 context switch에 4000ms (4초)로 오랜 시간이 걸림
  * 사용자 프로세스에 의해서 실제 사용되고 있는 메모리 크기를 정확히 파악하면 swapping에 걸리는 시간을 줄일 수 있음

<br>

* Swapping (Cont.)
  * swap out이 된 프로세스가 다시 swap in이 될때 원래의 메인 메모리 위치로 돌아와야 하는가?
    * 시스템의 bounding 방법에 따라 달라짐
  * 프로세스를 swapping 하려면 프로세스가 완전히 휴지 상태인지 확인해야함
  * 입출력은 항상 프로세스가 직접하지 않고 운영체제와 buffer와만 하게 함
    * double buffering 만 고려하면 overhead가 될 수 있음
  * 최근의 운영체제에서는 기본적인 swapping을 사용하지 않음
    * 기본적인 형태의 swapping 은 시간이 오래걸려서 합리적인 메모리 관리 정책이 될 수 없음
    * 평상시에는 swapping이 동작하지 않다가 메인 메모리의 빈공간이 일정 수준 이상으로 떨어지면 swapping이 동작하는 방법이 있음
    * 대부분의 swapping은 시스템의 가상 메모리 관리와 맞물려서 동작함

<br>

* Swapping on Mobile Systems
  * Mobile System은 실질적으로 어떠한 형태의 swapping 도 사용하지 않음
    * Mobile System은 Flash memory based로 저장 공간이 작고 물리적인 쓰기 횟수에 제한이 있어 swapping을 하기 적절하지 않음
  * swapping 대신에 다른 형태로 빈 메모리 공간을 확보함
    * iOS는 빈 메모리 공간 량이 일정수준 이하로 떨어지면 실행중인 앱들에게 이미 기존에 할당된 메모리를 반환을 하도록 요구하고 충분한 메모리를 반환하지 못한 응용은 강제 종료될 수 있음
    * Android에서는 iOS와 유사하게 메모리의 빈공간을 확보하고 프로그램을 강제 종료 시키는 것이 가능함 (프로그램 상태를 플래시 메모리에 저장함)
    * 두 운영체제는 paging을 지원하기 때문에 swapping을 하지 않아도 메모리 관리에 큰 문제가 없음

<br>

## 7.3 Contiguous Allocation

* Contiguous Allocation (연속 할당)
  * 메인메모리는 운영체제 자체 뿐만 아니라 여러 사용자 프로세스도 수용해야함
  * 메인메모리는 제한된 자원이기 때문에 각 영역이 목적에 맞게 효율적으로 관리 되어야함
  * 초창기 메모리 관리 기법 중에 하나인 Contiguous Allocation에 대해 알아봄
  * Contiguous Allocation은 메인 메모리를 두 영역으로 나눔
    * 커널 영역: 메인 메모리에 늘 상주하는 프로그램으로 주로 낮은 영역대에 자리잡고 있음
    * 사용자 프로세스 영역: 운영체제가 차지한 공간을 제외한 나머지 주소를 사용함

<br>

* Contiguous Allocation (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image06.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Relocation 레지스터를 사용하여 한 프로세스가 자신에게 주어진 영역 외에 다른 영역을 접근하는 것을 막음
  * CPU가 만들어내는 논리 주소를 limit 레지스터의 값과 비교하고 relocation 레지스터 값에 더해져서 최종 물리 주소로 변환되는 과정
  * 프로세스 마다 자신이 차지하는 영역의 시작주소가 relocation 레지스터의 값에 해당함
  * transient: 운영체제가 지원하던 주변 장치가 향후에 더 이상 사용되지 않게 되었다고 가정하면 운영체제에서 관련된 코드로 메모리에 존재할 필요가 없는 코드

<br>

* Multiple-partition allocation

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image07.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * Degree of multiprogramming 만큼 가용 메모리를 나누어 메모리를 할당함
  * 가변 크기 만큼의 partition이(Variable-partition) 사용됨
  * Hole: 어느 프로세스에도 할당되지 않은 메모리
  * 새로운 프로세스가 시작하게 되면 이 프로세스가 수용할 수 있는 충분한 크기의 Hole을 찾아서 할당함
  * 프로세스가 종료하면서 사용중인 메모리를 반환하게 되면 반환된 공간은 인접한 빈 공간이 있는 경우에 합쳐져서 유지됨
  * 운영체제는 프로세스에게 할당된 partition과 비어있는 Hole의 정보를 유지해야 함

<br>

* Dynamic Storage-Allocation Problem (여러 hole이 있고 크기 n의 메모리 요청이 있을 때 이를 어떻게 만족할 수 있을까?)
  * First-fit: hole 들 중에 검색하여 첫번째 사용가능한 빈 공간을 할당함
  * Best-fit: 요청을 수용할 수 있는 hole 들 중에서 가장 작은 hole을 할당함
  * Worst-fit: 요청을 수용할 수 있는 hole 들 중에서 가장 큰 hole을 할당함
  * First-fit이나 best-fit 이 Worst-fit 보다 성능이 좋음
* Fragmentation (단편화 현상)
  * External Fragmentation (외부 단편화): 주어진 메모리 요청에 의해서 현재 남은 hole들의 합이 요청을 만족할 수 있는 크기이지만 hole들이 흩어져 있어서 요청을 만족할 수 없는 경우
  * Internal Fragmentation (내부 단편화): 메모리 요청보다 큰 hole을 할당하는 과정에서 남은 공간
  * 어떤 할당 기법을 사용해도 메모리 전체에 hole들이 많아지고 이에 따라 Fragmentation이 발생하게 됨
    * First fit을 사용하여 N개의 block을 사용하면 0.5 N block은 fragmentaion으로 사용할 수 없음
* Fragmentation (Cont.)
  * 외부 단편화: 전체 메모리 공간에 대해서 서로 크기가 다른 여러 프로세스들이 나누어져 쌓인 경우 충분한 공간이 없어 프로그램 실행이 불가한 경우
    * Dynamic storage 기법을 사용하면 외부 단편화를 피할 수 없음
  * Compaction: 현재 프로세스들이 사용중인 메모리 블락들을 전체 주소 공간의 한쪽으로 압착시켜 공간을 만드는 방법
    * 단편화가 심해지면 compaction을 통해 낮출 수 있음
    * Compaction을 하면 기존의 동작중인 프로세스들이 원래 동작하고 있던 메모리 영역에서 이동하므로 relocation이 동적으로 이루어지는 시스템에서만 가능함
    * I/O problem: 입출력을 하고 있던 프로세스가 compaction을 할 때 해당 입출력의 buffer 위치가 달라질 수 있음
  * 메인 메모리 뿐만 아니라 디스크와 같은 backing store에도 단편화가 발생함

<br>

## 7.4 Segmentation

* Segmentation

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image08.PNG?raw=true" alt="image" style="zoom: 33%;" />

  * 사용자가 보는 메모리 관점에서 메인 메모리를 관리하는 방식
  * 하나의 프로그램은 일련의 세그먼트라는 집합으로 이루어짐
  * 세그먼트: 프로그램을 이루고 있는 논리적인 단위들 (stack, symbol table 등)

* Logical View of Segmentation (하나의 프로그램이 4개의 세그먼트로 이루어진 경우)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image09.PNG?raw=true" alt="image" style="zoom: 33%;" />

  * 세그먼트가 각각 코드, 스택, 데이터로 이루어져 있을때 각 할당 단위가 세그먼트가 됨

<br>

* Segmentation Architecture
  * 논리 주소의 구성: segment 번호와 offset
  * Segment table: 프로세스를 구성하는 세그먼트의 물리 메모리 상에서의 시작주소와 세그먼트의 크기가 기록됨
    * base: 물리 주소의 시작 주소
    * limit: 세그먼트의 크기
  * Segment-table base register (STBR): 현재 실행중인 프로세스의 물리 메모리 상의 시작위치를 가리킴
  * Segment-table length register (STLR): 현재 실행중인 프로세스의 세그먼트 개수를 갖고 있음

<br>

* Segmentation Architecture (Cont.)

  * Protection: 세그먼트의 단위로 이루어짐
    * validation bit: 세그먼트마다 해당 세그먼트가 valid한지 표시하는 비트
    * 해당 세그먼트에 대해서 할 수 있는 동작: read, write, execute privileges
  * 세그먼트마다 Protection 비트가 있고 프로세스 간의 메모리를 공유하려면 세그먼트 단위로 공유해야함
  * 세그먼트의 길이가 일정하지 않으므로 세그먼트 단위의 메모리 할당을 하면 storage-allocation problem이 발생함

* Segmentation Hardware

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image10.PNG?raw=true" alt="image" style="zoom: 33%;" />

<br>

## 7.5 Paging

* Paging

* Paging Model of Logical and Physical  Memory

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image11.PNG?raw=true" alt="image" style="zoom: 33%;" />

* Address Translation Scheme

  * Address generated by CPU is divided into:

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image12.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Paging Hardware

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image13.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Paging Example

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image14.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Paging (Cont.)

* Free Frames

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image15.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Implementation of Page Table

* Implementation of Page Table (Cont.)

  * Associative memory – parallel search 

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image16.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Paging Hardware With TLB

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image17.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Effective Access Time

* Memory Protection

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image18.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Shared Pages

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image19.PNG?raw=true" alt="image" style="zoom: 50%;" />

<br>

## 7.6 Structure of the Page Table

* Structure of the Page Table

* Hierarchical Page Tables

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image20.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Two-Level Paging Example

  * A logical address (on 32-bit machine with 1K page size) is divided into:

    * a page number consisting of 22 bits
    * a page offset consisting of 10 bits

  * Since the page table is paged, the page number is further divided into:

    * a 12-bit page number
    * a 10-bit page offset

  * Thus, a logical address is as follows:

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image21.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Address-Translation Scheme

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image22.PNG?raw=true" alt="image" style="zoom: 33%;" />

* 64-bit Logical Address Space

  *  Even two-level paging scheme not sufficient

  * If page size is 4 KB (212)

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image23.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Three-level Paging Scheme

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image24.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Hashed Page Tables

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image25.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Inverted Page Table

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image26.PNG?raw=true" alt="image" style="zoom: 50%;" />

<br>

## 7.7 Example: The Intel 32 and 64-bit Arc hitectures

* Example: The Intel 32 and 64-bit Architectures

* Example: The Intel IA-32 Architecture

  * Supports both segmentation and segmentation with paging

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image27.PNG?raw=true" alt="image" style="zoom: 50%;" />

  * IA-32 segmentation

* Example: The Intel IA-32 Architecture (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image28.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Example: The Intel IA-32 Architecture (Cont.)

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image29.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Intel IA-32 Page Address Extensions

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image30.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Intel x86-64

  *  Current generation Intel x86 architecture

  * 64 bits is ginormous (> 16 exabytes)

  * In practice only implement 48 bit addressing

    * Page sizes of 4 KB, 2 MB, 1 GB
    * Four levels of paging hierarchy

  * Can also use PAE so virtual addresses are 48 bits and physical  addresses are 52 bits

    <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image31.PNG?raw=true" alt="image" style="zoom: 50%;" />

* Example: ARM Architecture

  <img src="https://github.com/hyunmin0317/OS_Study/blob/main/image/chap07/image32.PNG?raw=true" alt="image" style="zoom: 50%;" />

  

