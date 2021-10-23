# CPU scheduling

- CPU burst : CPU의 명령을 실행하는 것 (IO가 많으면 burst가 짧다)

![cpu burst time](https://media.vlpt.us/images/dolarge/post/ef511da2-db86-4f8c-8e7a-6d76966e0af3/image.png)



- 가로축은 duration, 세로축은 빈도
  - IO bound가 많으면 자주 일어나고 IO bound가 없으면 한번에 길게 유지됨





## CPU Scheduler & Dispatcher

- CPU Scheduler
  - Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.
- Dispatcher
  - CPU 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다.
  - 이 과정이 context switching(문맥 교환)이다.



- CPU 스케쥴링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다.
  - 1. Running -> Blocked
    2. Running -> Ready
    3. Blocked -> Ready
    4. Terminate





## Scheduling Criteria, Performance Measure(= index) 성능 척도

### **시스템 입장에서 성능 척도**

- CPU utilization (이용률)
  - 전체 시간중에 CPU가 놀지 않고 일한 시간의 비율 = 최대한 많이 일해라

- Throughput(처리량)
  - 주어진 시간동안 얼마나 처리했는가



### **고객(프로세스) 입장에서 성능 척도**

- Turnaround time (소요시간, 반환시간)
  - CPU를 쓰러 들어와서 다 쓰고 나갈 때 까지 걸린 시간
  - Ready queue에 들어와서 처리가 끝날 때 까지의 시간

- Waiting time (대기시간)
  - 기다린 시간 (= Ready queue에서 기다린 시간)
- Response Time (응답 시간)
  - ready queue에서 처음으로 CPU를 얻기까지 걸린 시간



- 응답시간 <<< 대기시간 <<< 소요시간





## FCFS (First Come First Served)

- 먼저 온대로 순서대로 처리
- 비선점형 스케쥴링

- 짧은 프로세스가 먼저 도착하면 평균대기시간이 짧겠지만 긴 프로세스가 먼저 도착하면 평균 대기시간이 길어진다.

- Convoy Effect : 앞 쪽의 긴 프로세스 때문에 대기하는 프로세스들이 기다려야하는 현상





## SJF (Shortest-Job-First)

- CPU Burst 시간이 가장 짧은 프로세스 먼저 CPU 할당
- Optimal한 방법이다.

- 비선점한 상황
  - 먼저 도착한 CPU를 작업하다가 후에 도착한 짧은 프로세스 실행
- 선점한 상황
  - 수행중인 프로세스보다 짧은 프로세스가 도착하면 뺏어감



- 문제점

  - Starvation (기아 현상)

    - 계속 CPU burst가 짧은 프로세스가 들어오면 CPU burst가 긴 프로세스는 CPU할당을 못받는다.

    

  - CPU burst 시간을 예측 할 수 없음

    - 과거의 흔적을 통해 알 수는 있지만 항상 알 수 없다.





## Priority Scheduling

- 우선순위가 높은 프로세스에게 CPU 먼저 할당
- SJF도 일종의 priority scheduling 기법임
  - 우선순위가 가장 짧은 순서



- 문제
  - Starvation



- 해결책
  - Aging : 오래 기다린 프로세스에게 우선순위 할당





## RR (Round Robin)

- 현대적인 컴퓨터 시스템 스케쥴링은 RR에 기반함
- 각 프로세스는 동일한 크기와 할당 시간(time quantum)을 가짐
  - 일반적으로 10-100 m/s



- 할당 시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을섬

- 각 프로세스의 응답시간이 빨라진다. 최소한 할당된 시간만큼 모든 프로세스가 작업을 하기 때문.



- n개의 프로세스, 할당시간이 q time unit인 경우 어떠한 프로세스도 (n-1)q 시간만큼 기다리지 않음



- Performance
  - q large = FCFS
  - q small = context switch 오버헤드가 커진다.



- Response Time이 빨라지는게 장점



## Multilevel Queue

![multilevel](https://media.vlpt.us/images/dolarge/post/12d95331-398f-4113-8e75-342809e85fa1/image.png)

- 줄이 정해져 있고 프로세스가 우선순서대로 처리됨
- Ready queue를 여러개로 분할한 것
  - foreground queue (interactive한 job)
  - background queue (batch - no human interaction)



- 각 큐는 독립적인 스케쥴링 알고리즘을 가짐
  - foreground - RR
  - background - FCFS



- 큐에 대한 스케쥴링 필요
  - Fixed priority scheduling
    - foreground 작업 끝내고 background 작업 이루어짐
    - Starvation 가능성 존재
  - Time slice
    - 각 큐에 CPU time을 적절한 비율로 할당





## Multilevel Feedback Queue

![multilevel feedback queue](https://media.vlpt.us/images/dolarge/post/14edb3cf-efee-4865-86da-9f288c854425/image.png)

- 처음 도착하면 상위 큐로 들어가고 늦게 올수록 할당시간이 늦어지고 맨 아래는 FCFS 방식으로 적용
- 짧은 프로세스는 위로 보내고 긴 프로세스는 아래로 보내는 방식





## Multiple-processor Scheduling

- CPU가 여러 개인 경우 스케쥴링은 더욱 복잡해짐
- Homogeneous processor인 경우
  - queue에 한줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐



- Load sharing
  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
  - 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법



- Symmetric Multiprocessing (SMP)
  - 각 프로세서가 각자 알아서 스케쥴링 결정
  - 모든 CPU들이 대등한 상태



- Asymmetric multiprocessing
  - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름





## Real-time Scheduling

- Hard real-time scheduling
  - hard real-time task는 정해진 시간안에 반드시 끝내도록 스케줄링해야함
- Soft real-time scheduling
  - Soft real-time task 일반 프로세스에 비해 높은 priority를 갖도록 해야함
  - ex) 영화 시청
  - 데드라인을 꼭 보장 안해도 됨





## Thread Scheduling

- Local Scheduling
  - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케쥴할지 결정
- Global Scheduling
  - Kernel level thread의 경우 일반 프로세스와 마찬가지로 커널의 단기 스케쥴러가 어떤 thread를 스케쥴할지 결정





## Algorithm Evaluation

- Queuing models
  - 확률 분포로 주어지는 arrival rate와 service rate 등을 통해 각종 performance index 값을 계산



- Implementation (구현) & Measurement (성능 측정)
  - 실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대해서 성능을 측정 비교



- Simulation (모의 실험)
  - 알고리즘을 모의 프로그램으로 작성후 trace를 입력으로 하여 결과 비교