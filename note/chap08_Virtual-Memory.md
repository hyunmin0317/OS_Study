# chapter 8 : Virtual Memory

* ### Background

  * 프로그램 전체가 실행을 위해 memory에 있을 필요 X
  * 가상 메모리는 논리 주소와 물리 메모리 분리시키는 개념
  * 새로운 프로세스 생성 효율적 & 주소공간 공유 가능
  * 프로세스를 로드하거나 swapping 시 필요한 입출력 줄어듦
  * 가상 주소 공간 : 프로세스가 실행 중 메모리 상에서 어떻게 존재하는지 논리적인 view
  * demanding paging : 페이징 기반으로 페이지 필요시 메모리에 로드하는 가상메모리 기법
  * page table : 이 프로세스의 어느 페이지가 물리 메모리 어디에 있는지 정보 유지



* ### Demand paging

  * 개별 페이지 필요시 마다 하나 씩 로드
  * 장점 : I/O 최소화, 필요 메모리 감소, 응답시간 good, 더 많은 사용자 수용
  * pager : 페이지를 메모리로 가져오는 동작 담당 커널 모듈 - lazy swapper
    * valid, invalid bit
      * paging 에서는 주소공간에 속하는지 여부
      * demand paging에서는 메모리 로드 여부
    * page fault
      * valid가 invalid 인 경우 발생
      * 빈 프레임 확보 후 찾은 페이지 읽어들임
  * pure demand paging : 프로세스 페이지를 처음 시작 시 아무것도 메모리에 올려놓지 않은 상태에서 시작 
  * H/W 지원 : valid bit, 넓은 저장공간, 읽기, 쓰기 성능 지닌 보조기억장치, instruction restart
  * page fault 처리 시간
    * interrupt 처리 시간
    * page 디스크로부터 읽는데 걸리는 시간 - 제일 오래
    * 프로세스 재시작 시간
  * paging EAT = (1-p) * ma+p * page fault time
    * page fault table은 10% 미만이여야 함
  * 성능 최적화에는 swap space 관리 중요
    1. 전체 프로세스 모두 복사후 시작
    2. demand paging 하고 swap space에 기록

* ### copy on write

  * parent 프로세스가 child 생성 시 메모리 복제 대신 생성 초기에 부모 프로세스 페이지를 자식과 공유
  * 이후 쓰기 접근 시 복사 이루어짐
  * 프로세스 생성 시간 감소, 할당 페이지 수 감소
  * 빈 페이지 할당은 주로 zero fill ondemand
  * ufork : 자식이 부모 주소공간 사용

* ### page replacement

  * 빈프레임이 하나도 없는 상황 - 교체 필요
  * victim page : 교체 대상 페이지
  * 해당 페이지를 swap space에 쓰는 과정
  * 내용 수정 여부는 dirty bit 에 표시

* ### page and frame replacement algorithm

  1. FIFO

     * 가장 먼저 로드된 페이지 다음번 교체대상
     * frame 늘렸는데 page fault rate 증가 - Belady's Anomaly (3->4 : 9->10)
     * 큐 사용하여 구현

  2. optimal algorithm

     * 가장 오랫동안 사용되지 않을 페이지 교체
     * 구현 불가 (가장 최적)
     * 교체 알고리즘 성능 평가 기준

  3. LRU

     * 가장 오랫동안 참조되지 않았던 페이지 교체
     * 비교적 성능 good (FIFO 보다)
       1. counter algorithm
       2. stack

  4. LRU approximation

     * reference bit

     1. second chance (clock replacement)
        * 0인 페이지 교체, 1은 건너띄면서 0으로 클리어
        * circular queue 형태
     2. Enhanced second chance algorithm
        * modify bit (reference bit와 조합해 사용) - 수정 여부

  5. Counting algorithm - 별로 사용 X

     * 페이지별 참조 횟수 기록

     1. LFU : 참조횟수 가장 적은 페이지 교체 - 보안 : shift, 지수적으로 영향력 감소
     2. MFU : 참조횟수 가장 많은 페이지 교체

  6. Page buffering algorithm

     1. 일정 개수 빈 프레임 pool로 유지 - 읽어야 할 페이지 빈 프레임 중 하나로 읽기
     2. 수정된 페이지 리스트 유지 - disk 한가할 때 쓰기
     3. 빈 프레임 풀 유지하되 어떤 페이지 였는지까지 기억 

* ### Allocation of frames

  * 프레임 할당방법
    1. Fixed allocation
       * 프로세스마다 고정된 개수 프레임 할당
       * 전체 프레임 수 / 전체 프로세스 수 - 공평하게
    2. Priority allocation (fixed X - 가변적)
       * 프로세스 우선 순위 비례해서 할당
       * 페이지 교체에 영향 미칠 수 있음
  * global allocation
    * 페이지 교체 시 모든 프레임 대상으로 선택
    * 한 프로세스가 자신의 page fault율 완전히 조절 X
  * Local allocation
    * 할당된 프레임 수 변화 X

* ### thrashing

  * 과도한 페이지 동작이 이루어지는 현상
  * local replacement 나 priority replacement 사용 시 발생 제한
  * 프로세스 별 locality 합이 전체 메모리보다 크면 thrashing 발생
  * working set model - working set window 사용 - 델타 크기 적절히 조절해야 함
    * interval timer, reference bit 이용
  * page fault frequency
    * 한도 내의 page fault rat을 기준으로 상한과 하한 지정

* ### Memory mapped file

  * 하나의 디스크 블록을 메모리 상의 페이지로 매핑함으로써 파일 입출력을 메모리 접근처럼 함
  * shared memory 구현 가능

* ### Allocating kernel Memory

  * 사용자 프로세스가 추가 메모리 요구 시 커널이 자신이 관리하는 빈 프레임에서 빈 페이지 할당

  1. Buddy system
  2. Slab Allocator
