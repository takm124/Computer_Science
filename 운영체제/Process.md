# Process

[TOC]

## 프로세스의 개념

- Process is **a program in execution**



- 프로세스의 문맥(context)

  - CPU 수행 상태를 나타내는 하드웨어 문맥

    - Program Counter
    - 각종 Register

    

  - 프로세스의 주소 공간

    - code, data, stack

    

  - 프로세스 관련 커널 자료 구조

    - PCB (Process Control Block)
    - Kernel stack



- 프로세스의 문맥을 파악하고 있어야 다시 실행할 때 문제없이 재시작할수 있다.





## 프로세스의 상태 (Process State)

- 프로세스는 상태(state)가 변경되며 수행된다.

  - Running

    - CPU를 잡고 instruction을 수행중인 상태

    

  - Ready

    - CPU를 기다리는 상태(메모리 등 다른 조건을 모두 만족하고)
    - 다른 조건 = 프로세스 문맥을 다 알고 있어야함

    

  - Blocked (wait, sleep)

    - CPU를 주어도 당장 instruction을 수행할 수 없는 상태
    - Process 자신이 요청한 event가 즉시 만족되지 않아 이를 기다리는 상태
    - ex) 디스크에서 file을 읽어와야 하는 경우
    - 자신이 요청한 event가 만족되면 Ready 상태로 변함

    

  - Suspended (stopped)

    - 외부적인 이유로 프로세스의 수행이 정지된 상태

    - 프로세스는 통째로 디스크에 swap out된다.

    - 예) 사용자가 프로그램을 일시 정지시킨 경우(break key)

      메모리에 너무 많은 프로세스가 올라오면 잠시 정지됨

    - 외부에서 resume 해주어야 active가 됨

  

  - New : 프로세스가 생성중인 상태
  - Terminated : 수행(execution)이 끝남



![process lifecycle](https://i.imgur.com/Hbj2rEp.png)





- CPU queue나 IO queue는 Kernal Address Space의 Data 영역에 저장되어있음





## PCB (Process Control Block)

![PCB](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MDNfMTg0/MDAxNTkzNzQ1MjQ2Njgz.nmhyRLo_ygWgF9piW8BUwmCgcgq-W2HIeAW0Wxc1Kqcg.MS4ZFBQU7TGOFC1JtXVLkANPgrIyKgdb1tcp2laV6zQg.PNG.adamdoha/image.png?type=w800)

- 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보



- Pointer, Process State, Process Number
  - OS가 관리상 사용하는 정보
  - Process state (ready, running 등) , Process ID (프로세스 고유 번호)
  - scheduling information, priority
    - 우선순위나 스케쥴링 정보



- Program Counter, Registers
  - CPU 수행 관련 하드웨어 값



- Memory Limits
  - 메모리 관련
  - Code, data, Stack의 위치 정보



- List of open files
  - 파일 관련
  - Open file descriptors...





## Context Switch (컨텍스트 스위치)

- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행
  - CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
  - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴
  - 프로세스 PCB는 커널 스택에 저장되어 있음



- CPU는 PCB에서 PC와 Register 정보를 받아와 다시 실행함



- System call이나 Interrupt 발생시 반드시 Context Swtich가 일어나는 것은 아니다.

  - Process A가 실행중 ISR이나 System Call 함수로 Kernal mode로 잠시 변한 뒤 다시 프로세스 A가 실행 될 수 있음

  - Process A가 실행중 timer interrupt 혹은 IO 요청 system call이 오면 kernal 모드로 변환한 뒤 다른 프로세스로 넘어감(컨텍스트 스위치 일어남)

    - IO 요청은 오래걸리는 작업이라 컨텍스트 스위치 일어남

    



## 프로세스를 스케줄링하기 위한 큐

- Job queue

  - 현재 시스템 내에 있는 모든 프로세스의 집합

  

- Ready queue

  - 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합

  

- Device queue

  - I/O device의 처리를 기다리는 프로세스의 집합



- 프로세스들은 각 큐들을 오가며 수행됨



![프로세스 스케쥴링 큐](https://imbf.github.io/assets/computer-science/process-4.png)





## 스케쥴러 (Scheduler)

- 순서를 정해주는 역할



- Long-term scheduler (장기 스케쥴러 or Job scheduler)
  - 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정
    - new 에서 ready로 변하는 것을 결정(admitted) 
  - 프로세스에 memory(및 각종 자원)를 주는 문제
  - degree of Multiprogramming을 제어
    - 메모리에 올라가있는 프로세스의 수를 제어
  - time sharing system에는 보통 장기 스케줄러가 없음 (무조건 ready)



- Short-term scheduler (단기 스케줄러 or CPU scheduler)
  - 어떤 프로세스를 다음번에 running 시킬지 결정
  - 프로세스에 CPU를 주는 문제
  - 충분히 빨라야함 (m/s 단위)



- Medium-Term Scheduler (중기 스케줄러 or Swapper)
  - 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄
  - 프로세스에게서 memory를 뺏는 문제
  - degree of Multiprogramming을 제어
    - 메모리에 올라가있는 프로세스의 수를 제어



- 장기 스케줄러는 없기 때문에 중기 스케줄러가 메모리의 프로세스 수를 제어함



![프로세스 상태도](https://slid-capture.s3.ap-northeast-2.amazonaws.com/public/image_upload/56fb06df3c834488a9a7d8891d17fbac/43f5df5b-ba83-480c-91f7-8c2c47891f05.png)





## 쓰레드

- "A thread (or lightweight process) is a basic unit of CPU utilization"
- 프로세스에서 공유할 수 있는건 최대한 공유
  - PC, register, stack은 개별 보관
- Thread의 구성
  - program counter
    - 코드의 어느 부분을 실행하고 있는지 저장
  - register set
    - 어떤 값이 들어가있는가
  - stack space



- Thread가 동료 Thread와 공유하는 부분(= task)
  - code section
  - data section
  - OS resources



![멀티스레드](https://media.vlpt.us/images/holim0/post/4f7b6abd-5cbe-4039-9fb6-8cd1bc4ecaf5/4_01_ThreadDiagram.jpg)



## 스레드 장점

- 스레드 장점
  - 다중 스레드로 굿어된 태스크 구조에서는 하나의 서버 스레드가 blocked(wating) 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행(running)되어 빠른 처리 가능
  - 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughput)과 성능 향상을 얻을 수 있음
  - 스레드를 사용하면 병렬성을 높일 수 있음





- 응답성 (Responsiveness)
- 자원 공유(Resource Sharing)
- 경제성(Economy)
  - 새 프로세스를 만드는 것보다 스레드를 추가시키는 것이 낫다
  - 프로세스 간 context switch도 오버헤드가 큼
    - 오버헤드 =  어떤 처리를 하기 위해 들어가는 간접적인 처리 시간 · 메모리등

- 멀티 프로세스 구조의 유틸성





ref : http://www.kocw.net/home/cview.do?cid=3646706b4347ef09