# Disk Management and Scheduling

[TOC]



## Disk Structure

- logical block
  - 디스크의 외부에서 보는 디스크의단위 정보 저장공간들
  - 주소를 가진 1차원 배열처럼 취급
  - 정보를 전송하는 최소 단위



- Sector 
  - Logical block이 물리적인 디스크에 매핑된 위치
  - Sector 0은 최외곽 실린더의 첫 트랙에 있는 첫 번째 섹터이다.



## Disk Scheduling

![disk](https://media.vlpt.us/images/injoon2019/post/ab200b27-1181-46eb-80fd-dd2b1ff6e7c3/image.png)





## Disk Management

- physical formatting (low-level formatting)
  - 디스크를 컨트롤러가 읽고 쓸 수 있도록 섹터들로 나누는 과정
  - 각 섹터는 header + 실제 data (보통 512 bytes) + trailer로 구성
  - header와 trailer는 sector number, ECC (Error - Correctiong Code) 등의 정보가 저장되며 controller가 직접 접근 및 운영



- Partitioning

  - formatting이후의 과정

  - 디스크를 하나 이상의 실린더 그룹으로 나누는 과정
  - OS는 이것들 독립적 disk로 취급(logical disk)



- Logical formatting
  - 파일 시스템을 만드는 것
  - FAT, inode, free space 등의 구조 포함



- Booting
  - ROM에 있는 "small bootstrap loader"의 실행
  - sector 0 (boot block)을 load하여 실행
  - sector 0은 "full Bootstrap loader program"
  - OS를 디스크에서 load하여 실행



## Disk Scheduling Algorithm

- 큐에 다음과 같은 실린더 위치의 요청이 존재하는 경우 디스크 헤드 53번에서 시작한 각 알고리즘의 수행 결과는? (실린더 위치는 0-199)
- 98, 183, 37, 122, 14, 124, 65, 67





- FCFS
- SSTF
- SCAN
- C-SCAN
- N-SCAN
- LOOK
- C-LOOK



## FCFS (First come Firest Served)

- 들어온 요청 순서대로 처리
- 대단히 비효율적

![fcfs](https://media.vlpt.us/images/injoon2019/post/68696683-df2e-48b2-a6b8-283f4a3cd52a/image.png)





## SSTF(Shortest Seek Time First)

- 현재 헤드위치에서 가장 가까운 요청 처리

![SSTF](https://media.vlpt.us/images/injoon2019/post/5553014f-31a0-4e58-8bde-4f1320efeead/image.png)





## SCAN

![scan](https://media.vlpt.us/images/injoon2019/post/7c46bb5e-33b3-4b1f-a5c7-1e5f935637e8/image.png)





## C-SCAN

![C-scan 1](https://media.vlpt.us/images/injoon2019/post/977bf582-2e8b-4f60-ab42-65afea36217e/image.png)

![C-scan 2](https://media.vlpt.us/images/injoon2019/post/b2f02293-7fde-46f0-ae49-04e3bb5049c3/image.png)





## Other - Algorithm

- N-SCAN
  - SCAN의 변형 알고리즘
  - 일단 arm이 한 방향으로 움직이기 시작하면 그 시점 이후에 도착한 job은 되돌아올 때 service



- LOOK and C-LOOK
  - SCAN이나 C-SCAN은 헤드가 디스크 끝에서 끝으로 이동
  - LOOK과 C-LOOK은 헤드가 진행 중이다가 그 방향에 더 이상 기다리는 요청이 없으면 헤드의 이동방향을 즉시 반대로 이동한다.



![other](https://media.vlpt.us/images/injoon2019/post/3af27740-8852-462d-bde5-1aa1af3e8a97/image.png)



## Disk-Scheduling Algorithm의 결정

- SCAN, C-SCAN 및 그 응용 알고리즘은 LOOK, C-LOOK 등이 일반적으로 디스크 입출력이 많은 시스템에서 효율적인것으로 알려져 있음



- File의 할당 방법에 따라 디스크 요청이 영향을 받음



- 디스크 스케줄링 알고리즘은 필요할 경우 다른 알고리즘으로 쉽게 교체할 수 있도록 OS와 별도의 모듈로 작성되는것이 바람직하다.



