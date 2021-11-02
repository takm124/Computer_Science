# Deadlock



## Deadlock problem

- Deadlock
  - 일련의 프로세스들이 서로가 가진 자원을 기다리며 block 된 상태



- Resource (자원)
  - 하드웨어, 소프트웨어 등을 포함하는 개념
    - I/O device, CPU cycle, memory space, semaphore 등
  - 프로세스가 자원을 사용하는 절차
    - Request, Allocate, Use, Release



## Deadlock 발생 조건 4가지

- Mutual exlusion (상호 배제)
  - 매 순간 하나의 프로세스만이 자원을 사용할 수 있음



- No preemption (비선점)
  - 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음



- Hold and wait (보유 대기)
  - 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음



- Circular wait (순환 대기)
  - 자원을 기다리는 프로세스간에 사이클이 형성되야 함
  - 꼬리에 꼬리를 무는 형태로 대기





## Deadlock 처리 방법

### 미연에 방지

- Deadlock Prevention
  - 자원 할당시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는것



- Deadlock Avodiance
  - 자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당
  - 시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원 할당



### 방지 x - 데드락은 빈번하게 일어나는 이벤트가 아니다.

- Deadlock Detection and recovery
  - Deadlock 발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견시 recover



- Deadlock Ignorance
  - Deadlock을 시스템이 책임지지 않음
  - UNIX를 포함한 대부분의 OS가 채택





## Deadlock Prevention

- Mutual Exclusion
  - 공유해서는 안되는 자원의 경우 반드시 성립해야 함
  - 건들 수 없는 조건임



- Hold and Wait
  - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다.
  - 방법 1. 프로세스 시작 시 모든 필요한 자원을 할당받게 하는 방법 (처음에 다 hold하고 시작)
    - 자원의 비효율성이 생길 수 있음
  - 방법 2. 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청



- No Preemption
  - process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
  - 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다.
  - State를 쉽게 save하고 resotre할 수 있는 자원에서 주로 사용 (CPU, Memory)



- Circular Wait
  - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당
  - 예를 들어 순서가 3인 자원 R(i)를 보유 중인 프로세스가 순서가 1인 자원 R(j)를 할당받기 위해서는 우선 R(i)를 release 해야함
    - Utilization  저하, Throughput 감소, starvation 문제



## Deadlock Avoidance

- 최악의 상황을 가정함

- 자원 요청에 대한 부가정보를 이용해서 자원 할당이 deadlock으로 부터 안전(safe)한지를 동적으로 조사해서 안전한 경우에만 할당
- 가장 단순하고 일반적인 모델은 프로세스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법임



- safe state
  - 시스템 내의 프로세스들에 대한 safe sequence가 존재하는 상태



- safe sequence
  - 프로세스의 sequence <P1, P2 ... Pn>이 safe 하려면 P(i)의자원 요청이 "가용 자원 + 모든 P(j)(j < i)의 보유 자원"에 의해 충족되어야 함



- 시스템이 safe state에 있으면 no deadlock
- 시스템이 unsafe state에 있으면 possibility of deadlock



- 2가지 경우의 avoidance 알고리즘
  - single instance per resource types
    - Resource Allocation Graph algorithm 사용
  - Multiple instances per resource types
    - Banker's Algorithm 사용





### Resouce Allocation Graph Algorithm

![Resouce Allocation Graph Algorithm](https://media.vlpt.us/images/injoon2019/post/d376a39a-7a7f-4f36-b190-24e4816003fd/image.png)

- 실선 : 요청
- 점선 : 요청할 수 도 있음

- 사이클이 생성될 것 같으면 할당 안해줌



### Banker's Algorithm

- 가용 자원으로 해결할 수 있는 상태를 찾음
  - 항상 safe를 유지하려고 함



![Banker's Algorithm](https://media.vlpt.us/images/injoon2019/post/da5ac659-6a41-41d3-a851-364e1e9257f6/image.png)

![banker 2](https://media.vlpt.us/images/injoon2019/post/67446156-b114-41a9-99ec-02427a3954d9/image.png)



- 자원이 남아도는데도 안주는거임 -> 비효율적



### Deadlock Detection and Recovery

![detection](https://media.vlpt.us/images/injoon2019/post/2de78c01-db61-4c60-b29c-b512629ccfaa/image.png)

- 자원 할당 그래프를 wati-for graph로 바꿈
- 사진에서 사이클이 있기 때문에 데드락인 상황임



- Deadlock Detection
  - Resource type 당 single instacne인 경우
    - 자원 할당 그래프에서 cycle이 곧 deadlock을 의미
  - Resouce type 당 multiple instance인 경우
    - Banker's algorithm과 유사한 방법 활용



- Wait-for graph 알고리즘
  - Resource type 당 single instance인 경우
  - Wait-for graph
    - 자원할당 그래프의 변경
    - 프로세스만으로 node 구성
    - P(j)가 가지고 있는 자원을 P(k)가 기다리는 경우 P(k) -> P(j)
  - Algorithm
    - Wait-for graph에 사이클이 존재하는지를 주기적으로 조사
    - O(n^2)



- Recovery

  - Process termination

    - Abort all deadlocked processes
    - Abort one process at a time until the deadlock cycle is eliminated

    

  - Resource Preemtion

    - 비용을 최소화할 victim의 선정
    - safe state로 rollback하여 process를 restart
    - Starvation 문제
      - 동일한 프로세스가 계속해서 victim으로 선정되는 경우
      - cost factor에 rollback 횟수도 같이 고려