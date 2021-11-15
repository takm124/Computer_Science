# File System Implementation

[TOC]



## Allocation of File Data in Disk

- Contiguous Allocation
- Linked Allocation
- Indexed Allocation





## Contiguous Allocation

![Contiguous Allocation](https://media.vlpt.us/images/injoon2019/post/02ce7742-2e3b-4153-9b1b-e4f67d7b4bb2/image.png)

- 임의의 크기의 파일을 블럭 단위로 나누어 저장

- 디스크에 연속적으로 저장
- Directory에 담겨진 것이 file의 metadata



- 단점
  - external fragmentation
  - File grow가 어려움 (파일 크기 변동에 제한이 있음)
    - file 생성시 얼마나 큰 hole을 배당할 것인가?
    - grow 가능 vs 낭비 (internal fragmentation)



- 장점
  - Fast I/O
    - 한번의 seek/rotation으로 많은 바이트 transfer
    - Realtime file 용으로, 또는 이미 run 중이던 process의 swapping 용
  - Direct access (=random access) 가능







## Linked Allocation

![linked allocation](https://media.vlpt.us/images/injoon2019/post/a1ae4ddd-c6ff-426c-b523-89f5f536f4d0/image.png)



- 장점
  - External fragmentation 발생 안 함



- 단점
  - No random access
  - Reliability 문제
    - 한 sector가 고장나 pointer가 유실되면 많은 부분을 잃음
  - Pointer를 위한 공간이 block의 일부가 되어 공간 효율성을 떨어트림
    - 512 bytes/sector, 4 bytes/pointer



- 변형
  - File-Allocation table (FAT)파일 시스템
    - 포인터를 별도의 위치에 보관하여 reliability와 공간 효율성 문제 해결





## Indexed Allocation

![indexed allocation](https://media.vlpt.us/images/injoon2019/post/44b23272-9fed-4ded-a7a1-a90682ac770e/image.png)

- directory에 index 하나를 저장하고 index에 파일 위치 정보를 모두 저장함



- 장점
  - External fragmentation 발생하지 않음
  - Direct access 가능



- 단점

  - Small file의 경우 공간 낭비(실제로 많은 file들이 small)

  - Too large file의 경우 하나의 block으로 index를 저장하기에 부족

    - 해결 방안

    1. linked scheme
    2. multi-level index







# 여기서부터 실질적인 메모리할당

## UNIX 파일 시스템의 구조

![unix 1](https://media.vlpt.us/images/injoon2019/post/3c866bf2-a356-467e-a401-ad12df85e240/image.png)

![unix 2](https://media.vlpt.us/images/injoon2019/post/5ea798f2-ee1d-4041-ad3c-ad9e98489549/image.png)



- Boot block이 항상 제일 앞에 나옴, 0번 블록 (부팅을 해야하니까)

- superblock은 어디가 빈 블록이고 어디가 실제로 사용중인 블록인지 관리함

- Directory가 file에 대한 모든 정보를 보관하는 것은 아님, Inode에도 저장됨





## FAT File System

![FAT](https://media.vlpt.us/images/injoon2019/post/e562695d-d24e-42d6-af2e-253927ba8b61/image.png)



- Boot block은 여기서도 0번 블록
- 메타데이터 일부를 FAT에 저장함 (지극히 제한적, 위치정보만)
  - 나머지는 directory가 저장

- 다음 블럭의 위치를 FAT에 저장하기 때문에 bad sector등의 문제로 발생할 수 있는 단점을 극복함
  - linked allocation의 상위호환





## Free-Space Management

- 비어있는 블록들에 대한 관리



![free space1](https://media.vlpt.us/images/injoon2019/post/96bee84c-ba22-4cd7-aeb9-ab5d9864a3ce/image.png)

![free space2](https://media.vlpt.us/images/injoon2019/post/63c1e652-cbb1-4fbb-9a57-badbb1d6e9f4/image.png)



- 연속적인 빈 block을 찾기에는 Counting이 제일 효율적
  - 빈 블럭의 위치를 가리키고 갯수 정보도 포함함





## Directory Implementation

![directory implementation](https://media.vlpt.us/images/injoon2019/post/3aa9d292-c39c-4f37-a2d5-91fb92b61222/image.png)

![DI](https://media.vlpt.us/images/injoon2019/post/ec832c3c-c34b-4427-a4d6-f76afcc8e5d7/image.png)





## VFS and NFS

- Virtual File System (VFS)
  - 서로 다른 다양한 file system에 대해 동일한 시스템 콜 인터페이스 (API)를 통해 접근할 수 있게 해주는 OS의 layer



- Network File System (NFS)
  - 분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있음
  - NFS는 분산 환경에서의 대푲거인 파일 공유 방법임



![VFS](https://media.vlpt.us/images/injoon2019/post/a235d173-60ec-4291-a429-e36332d66f30/image.png)

- 보통은 VFS를 다 씀 (개별 파일 시스템과 상관없이)



## Page Cache and Buffer Cache

![Cache](https://media.vlpt.us/images/injoon2019/post/a55ae17a-5d67-45ea-8af1-f708756b4400/image.png)



- Page Cache
  - virtual memory의 paging system에서 사용하는 page frame을 caching의 관점에서 설명하는 용어
  - Memory-Mapped I/O를 쓰는 경우 file의 I/O에서도 page cache 사용



- Memory-Mapped I/O
  - File의 일부를 virtual memory에 mapping 시킴
  - 매핑시킨 영역에 대한 메모리 접근 연산은 파일의 입출력을 수행하게 함



- Buffer Cache
  - 파일시스템을 통한 I/O 연산은 메모리의 특정 영역인 buffer cache 사용
  - File 사용의 locality 활용
    - 한번 읽어온 block에 대한 후속 요청 시 buffer cache에서 즉시 전달
  - 모든 프로세스가 공용으로 사용
  - Replacement algorithm 필요(LRU, LFU 등)



- Unified Buffer Cache
  - 최근의 OS에서는 기존의 buffer cache가 page cache에 통합됨



![cache2](https://media.vlpt.us/images/injoon2019/post/82bed695-aab9-4503-bddc-2a03eee0e6d4/image.png)











사진 출처 : https://velog.io/@injoon2019/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EB%B0%98%ED%9A%A8%EA%B2%BD-%EA%B5%90%EC%88%98%EB%8B%98-2017%EB%85%84-12.-%ED%8C%8C%EC%9D%BC%EC%8B%9C%EC%8A%A4%ED%85%9C



ref : http://www.kocw.net/home/cview.do?cid=3646706b4347ef09 (운영체제, 반효경 교수)