 >6.1 CPU 스케쥴러
  - 준비 상태에 있는 프로세스들 중 어떠한 프로세스에게 CPU를 할당할지 결정하는 운영체제의 코드
  - cpu 스케줄러가 필요한 상황 (호출되는 상황)
    1. 실행 상태에 있던 프로세스가 봉쇄(blocked) 상태로 바뀌는 경우 (비선점)
    2. 실행 상태에 있던 프로세스가 타이머 인터럽트 발생에 의해 준비 상태로 바뀌는 경우 (선점)
    3. I/O 요청으로 봉쇄 상태에 있던 프로세스의 I/O 작업이 완료되어 인터럽트가 발생하고 그 결과 이 프로세스의 상태가 준비상태로 바뀌는경우 (선점)
    4. CPU에서 실행 상태에 있는 프로세스가 종료되는 경우 (비선점)
  - CPU 스케줄링 방식
    - 비선점형 : 스스로 반납하기 전까지는 반납 x
    - 선점형 : CPU 빼앗을 수 있음
    

<br>

 >6.2 디스패처 (dispatcher)
  - CPU가 할당되도록 결정이되면 이양되는 작업이 필요하다.
  - 새롭게 선택된 프로세스가 CPU를 할당받고 작업을 수행할 수 있도록 환경설정을 하는 운영체제의 코드를 디스패처라 부름
  - 현재 실행중인 프로세스의 문맥을 pcb에 저장하고 새롭게 선택된 프로세스의 pcb를 복원하여 프로세스에게 cpu를 넘김


<br>

>6.3 스케줄링의 성능 평가
  - 스케줄링 기법의 성능을 평가하기 위한 지표
    - 시스템 관점 : CPU 이용률, 처리량
    - 사용자 관점 : 소요시간, 대기시간, 응답시간
  - CPU 이용률 : 전체 시간 중 cpu가 일을한 시간의 비율
  - 처리량 : 주어진 시간 동안 준비 큐에서 기다리고 있는 프로세스 중 몇개를 끝냈는지
  - 소요시간 : 프로세스가 cpu를 요청한 시점부터 자신이 원하는 만큼 cpu를 다 쓰고 cpu 버스트가 끝날 때까지 걸린 시간
  - 대기시간 : 준비큐에서 cpu를 얻기위해 기다린 시간
  - 응답시간 : 준비 큐에 들어온 후 첫번째 cpu를 획득하기 위해 기다린 시간
    

<br>

>6.4 스케줄링 알고리즘
  - 선입선출 (First Come, First Out)
  - 최단 작업 우선 스케쥴링 (Shortest-job First)
  - 우선 순위 스케쥴링 (priority scheduling)
  - 라운드 로빈 스케쥴링 (round-robin)
  - 멀티레벨 큐
  - 멀티레벨 피드백 큐
  - 다중 처리기 스케쥴링
  - 실시간 스케쥴링
  

<br>

>6.5 스케줄링 알고리즘의 평가
  - 스케쥴링 알고리즘의 성능을 평가하는 방법
    - 큐잉 모델
    - 시뮬레이션
    - 구현 및 실측


<br>

