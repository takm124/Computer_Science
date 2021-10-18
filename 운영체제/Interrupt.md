## Interrupt

- 현대의 운영체제는 인터럽트에 의해 구동됨

- CPU의 Inturrupt line에 인터럽트가 들어옴
- 인터럽트 당한 시점의 레지스터와 program counter(PC)를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘김



- 시스템 콜
  - 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것
  - 사용자 프로그램이 운영체제에게 부탁하는 것(IO를 사용하기 위해, **특권명령**)



- Interrupt (넓은 의미)
  - Interrupt (하드웨어 인터럽트) : 하드웨어가 발생시킨 인터럽트 -> 진정한 의미의 인터럽트라 할 수 있다.
  - Trap (소프트웨어 인터럽트) 
    - Exception : 프로그램이 오류를 범한 경우
    - System call : 프로그램이 커널 함수를 호출하는 경우



- 인터럽트 우선 순위 (높은 순)
  - 1. 전원 이상 (Power fail)
    2. 기계 고장(Machine Chekc) - 하드웨어
    3. 외부 신호(External)
    4. 프로그램 검사(Program Check) - overflow, underflow
    5. SCV(SuperVisor Call) - 제어 프로그램 호출



- 인터럽트 관련 용어
  - 인터럽트 벡터
    - 해당 인터럽트의 처리 루틴 주소를 가지고 있음
  - 인터럽트 처리 루틴 (Interrupt Service Routine, 인터럽트 핸들러)
    - 해당 인터럽트를 처리하는 커널 함수



- 처리과정

-![인터럽트 처리 과정](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile7.uf.tistory.com%2Fimage%2F990473465CBF4858039C5C)





### Mode bit

- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치
- 1 사용자 모드 : 사용자 프로그램 수행
- 0 모니터 모드 : OS 코드 수행
  - 모니터 모드 = 커널모드, 시스템 모드



- 보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행 가능한 '특권 명령'으로 규정
- interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 바꿈
- 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로셋팅





- 즉, 관리자가 접근하냐, 일반 사용자가 접근하냐를 구분한다는 것

