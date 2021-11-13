# Virtual Memory

- 전적으로 운영체제가 관여함 (MMU는 관여 안함)

- 대부분의 시스템은 페이징 기법을 사용함



## Demand Paging

- 실제로 필요할 때 page를 메모리에 올리는 것
  - I/O 양의 감소
  - Memory 사용량 감소
  - 빠른 응답 시간
  - 더 많은 사용자 수용



- Valid / Invalid bit 사용
  - Invalid
    - 사용되지 않는 주소 영역인 경우
    - 페이지가 물리적 메모리에 없는 경우 (swap area에 내려가 있는 경우 (=backing store))
  - 처음에는 모든 page entry가 invalid로 초기화
  - address translation 시에 invalid bit이 set되어 있으면 => page fault



![page table](https://user-images.githubusercontent.com/37871541/78536382-b80b6180-7828-11ea-9117-f86dfd77f2e1.png)



## Page Fault

- invalid page를 접근하면 MMU가 trap을 발생 시킴 (page fault trap, 일종의 interrupt)
- Kernel mode로 들어가서 page fault handler가 invoke됨
- 다음과 같은 순서로 page fault 처리
  1. Invalid reference ? => abort process
  2. Get an empty page frame (없으면 뺏어온다 : replace)
  3. 해당 페이지를 disk에서 memory로 읽어온다.
     1. disk I/O가 끝나기까지 이 프로세스는 cpu를 preempt 당함(block)
     2. Disk read가 끝나면 page tables entry 기록, valid/invalid bit = "valid"
     3. ready queue에 process를 insert -> dispatch later
  4. 이 프로세스가 CPU를 잡고 다시 Running
  5. 아까 중단되었던 instruction을 재개



![page fault](https://user-images.githubusercontent.com/37871541/78536519-f012a480-7828-11ea-9b4e-1fd1c7e3c7c4.png)





## Free frame이 없는 경우

- Page replacement

  - 어떤 frame을 빼앗아올지 결정해야함

  - 곧바로 사용되지 않을 page를 쫓아내는 것이 좋음

  - 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 들어올 수 있음

    (쫓겨난 페이지 = victim page)



- Replacement Algorithm
  - page-fault rate를 최소화하는 것이 목표
  - 알고리즘의 평가
    - 주어진 page reference string에 대해 page fault를 얼마나 내는지 조사
  - FIFIO : 먼저 들어온 것을 먼저 내쫓음
    - 프레임이 많다고 page faults가 적게 일어나는 것은 또 아님
  - LRU : 가장 오래전에 참조된 것을 내쫓음
  - LFU : 참조 횟수가 가장 적은 페이지를 지움



![page change](https://user-images.githubusercontent.com/37871541/78538479-066e2f80-782c-11ea-97e6-94ed4e8ee699.png)



## 다양한 캐싱 환경

- 캐싱 기법

  - 한정된 빠른 공간(=캐시)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로부터 직접 서비스하는 방식
  - paging system 외에도 cache memory, buffer caching, Web caching 등 다양한 분야에서 사용

  

- 캐쉬 운영의 시간 제약

  - 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음
  - buffer caching이나 web caching의 경우
    - O(1)에서 O(log n)정도 까지 허용
  - Paging system인 경우
    - page fault인 경우에만 os가 관여함
    - 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 os가 알 수 없음
    - O(1)인 LRU의 list 조작조차 불가능



## Clock Algorithm

- LRU의 근사(approximation) 알고리즘
- 여러 명칭으로 불림
  - second chance algorithm
  - NUR(Not Used Recently), NRU (Not Recently used)
  - Reference bit를 사용해서 교체 대상 페이지 선정(circular list)
  - reference bit가 0인 것을 찾을 때까지 포인터를 하나씩 앞으로 이동
  - 포인터 이동하는 중에 refence bit 1은 모두 0으로 바꿈
  - reference bit이 0인 것을 찾으면 그 페이지를 교체
  - 한 바퀴 되돌아와서도(=second chance) 0이면 그때에는 replace당함
  - 자주 사용되는 페이지라면 second chacnce가 올때 1 유지 할 것



- Clock algorithm의 개선
  - reference bit과 modified bit(dirty bit)을 함께 사용
  - reference bit = 1 : 최근에 참조된 페이지
  - modified bit = 1 : 최근에 변경된 페이지(I/O를 동반하는 페이지)



![clock algorithm](https://t1.daumcdn.net/cfile/tistory/99B70C355B699CF61B)





## Page frame의 Allocation

- Allocation problem : 각 process에 얼마만큼의 page frame을 할당할 것인가?
- Allocation의 필요성
  - 메모리 참조 명령어 수행시 명령어, 데이터 등 여러 페이지 동시 참조
    - 명령어 수행을 위해 최소한 할당되어야 하는 frame의 수가 있음
  - Loop를 구성하는 page들은 한꺼번에 allocate 되는 것이 유리함
    - 최소한의 allocation이 없으면 매 loop 마다 page fault



- Allocation Scheme
  - Equal allocation : 모든 프로세스에 똑같은 갯수 할당
  - Proportional allocation ; 프로세스 크기에 비례하여 할당
  - Priority allocation : 프로세스의 priority에 따라 다르게 할당





## Thrashing

- 프로세스의 원활한 수행에 필요한 최소한의 page frame 수를 할당 받지 못한 경우 발생
- Page fault rate 매우 높아짐
- CPU utilization 낮아짐
- OS 또는 MPD(Multiprogramming degree)를 높여야 한다고 판단
- 또 다른 프로세스가 시스템에 추가됨 (higher MPD)
- 프로세스 당 할당된 frame의 수가 더욱 감소
- 프로세스는 page의 swap in / out 으로 매우 바쁨
- 대부분의 시간에 CPU는 한가함
- low throughput



![thrashing](https://1.bp.blogspot.com/-9byR8DLt8ZA/XWSyF_y1Q2I/AAAAAAAABi0/SgkGhByW8hwqD2OhqQuU7eFF208-dPsCwCLcBGAs/s1600/%25EC%25BA%25A1%25EC%25B2%2598.JPG)





## Working-Set Model

- Locality of reference
  - 프로세스는 특정 시간동안 일정 장소만을 집중적으로 참조한다.
  - 집중적으로 참조되는 해당 page들의 집합을 locality set이라 함



- Wokring set model
  - Locality에 기반하여 프로세스가 일정 시간 동안 훨활하게 수행되기 위해 한꺼번에 메모리에 올라와 있어야 하는 page들의 집합을 working set이라 정의함
  - Wokring set 모델에서는 process의 working set 전체가 메모리에 올라와 있어야 수행되고 그렇지 않을 경우 모든 frame을 반납한 후 swap out (suspend)
  - Thrashing을 방지함
  - multiprogramming degree를 결정함



![working set](https://jhi93.github.io/assets/img/os/WorkingSetAlgorithm.png)





## PFF

![page fault frequency](https://jhi93.github.io/assets/img/os/PFFScheme.png)



## Page Size의 결정

- page size를 감소시키면
  - 페이지 수 증가
  - 페이지 테이블 크기 증가
  - internal fragmentation 감소
  - Disk transfer의 효율성 감소
    - seek/rotation vs transfer
  - 필요한 정보만 메모리에 올라와 메모리 이용이 효율적
    - locality 활용 측면에서는 좋지 않음



- Trend
  - Larger page size