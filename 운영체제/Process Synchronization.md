# Process Synchronization

[TOC]

- 컴퓨터 시스템 내에서 데이터가 접근되는 패턴
  - 데이터가 저장되어있는 위치가 있음
  - 데이터를 읽어와서 연산을 하고 결과를 Return 함
  - 즉, 데이터가 저장되어있는 위치와 연산하는 위치가 따로 있음
  - '내가' 데이터를 수정하는 사이 기존 데이터가 변경되면서 문제가 발생할 수 있음



- 결론 :  멀티프로세서 시스템에서 공유메모리를 사용하는 프로세스들 사이에서 문제가 발생 할 수 있다.





## Race Condition(경쟁 상태)

- Kernel 수행 중 인터럽트 발생 시

  - 양쪽 다 커널 코드이므로 kernel address space 공유

  

- Process가 system call을 하여 kernel mode로 수행중인데 context switch가 일어나는 경우

  - 해결책 : 커널 모드에서 수행중일 때는 CPU를 선점하지 않음

    커널 모드에서 사용자 모드로 돌아갈때 선점(preempt)



- Multiprocessor에서 shared memory 내의 kernel data
  - 앞의 해결책으로 해결이 안되는 상황임
  - 방법 1 : 한번에 하나의 CPU만이 커널에 들어갈 수 있게 하는 방법
  - 방법 2 : 커널 내부에 있는 각 공유데이터에 접근할 때마다 그 데이터에 대한 lock/unlock을 하는 방법



## Process Synchronization 문제

- 공유 데이터(Shared data)의 동시 접근(concurrent access)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다.
- 일관성(consistency) 유지를 위해서는 협력 프로세스(cooperating process)간의 실행 순서(orderly execution)를 정해주는 메커니즘 필요



- Race condition
  - 여러 프로세스들이 동시에 공유데이터를 접근하는 상황
  - 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐



- Race Condition을 막기 위해서는 concurrent process는 동기화(synchronize) 되어야 한다.





## Critical-Section

- 공유데이터를 접근하는 코드를 의미함
- n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
- 각 프로세스의 code segment에는 공유데이터를 접근하는 코드인 critical section 존재
- 문제점
  - 하나의 프로세스가 critical section에 있을때 다른 모든 프로세스는 critical section으로 들어갈 수 없어야 한다.





## 문제 해결의 시작

- 두 개의 프로세스가 있다고 가정 P0, P1
- 프로세스들의 일반적인 구조

```c
do {
    entry section
    critical section
    exit section
    remainder section
} while (1);
```



- 프로세스들은 수행의 동기화(synchronize)를 위해 몇몇 변수를 공유할 수 있다.



- 프로그램적 해결법의 충족 조건
  - Mutual Exclusion (상호 배제)
    - 프로세스 P가 critical section 부분을 수행중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안된다.
  - Progress
    - 아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있으면 critical section에 들어가게 해주어야 한다.
  - Bounded waiting
    - 프로세스가 critical section에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다.



- 가정
  - 모든 프로세스의 수행 속도는 0보다 크다.
  - 프로세스들 간의 상대적인 수행 속도는 가정하지 않는다.



## Semaphores (추상 자료형)

- Semaphore S

  - Integer variable -> 자원의 갯수

  - 아래의 두 가지 atomic 연산에 의해서만 접근 가능

    ```c
    P(S) while (S<=0) do no-op; --> 남는 자원이 없으면 대기
         s--; --> 자원 생기면 가져감
         
    V(S) : S++; --> 자원 반납
    ```

  - P 연산 : 세마포어 변수 값을 획득하는 과정

  - V 연산 : 반납 과정



- Busy-waiting 방식은 효율적이지 못함 (= spin lock)



## Block / Wake Up Implementation

- 세마포어 구현 방식 중 하나
- block : 커널은 block을 호출한 프로세스를 suspend 시킨다. 이 프로세스의 PCB를 semaphore에 대한 wait queue에 넣는다.
- wakeup (P) : block된 프로세스 P를 wakeup 시킴. 이 프로세스의 PCB를 ready queue로 옮긴다.

![block / wakeup](https://media.vlpt.us/images/dolarge/post/d5a97c59-27fb-420b-ba04-d99e8d3c8008/image.png)



- P 연산
  - 자원을 얻는 연산
  - 값이 음수면(자원의 여분이 없다면) List에 넣고 block 시켜 놓음



- V 연산
  - 자원을 내놓아도 S가 0 이하라면 기다리는 프로세스가 있다는 것
  - 있으면 깨워준다.



- busy - waiting보다 block/wake up 방식이 좀더 낫긴 함

  - 쓸 데 없는 CPU 낭비가 없기 때문

  - critical section의 길이가 긴 경우는 block/wakeup이 적당 

    길이가 짧은경우에는 block/wakeup의 오버헤드가 busy-waiting오버헤드보다 더 커질수 있음





## 세마포어의 두가지 타입

- Counting semaphore
  - 도메인이 0 이상인 임의의 정수값
  - 주로 resource counting에 사용



- Binary semaphore (= mutex)
  - 0 또는 1 값만 가질 수 있는 semaphore
  - 주로 mutual exclusion (lock/unlock)에 사용



## 세마포어가 발생시킬 수 있는 문제

- DeadLock
  - 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상



- Starvation

  - indefinite blocking. 프로세스가 suspend된 이유에 해당하는 세마포어 큐에서 빠져나갈 수 없는 현상

    