# File System

[TOC]

`

## File and File system

- File
  - "A named collection of related information"
  - 일반적으로 비휘발성의 보조기억장치에 저장
  - 운영체제는 다양한 저장 장치를 file이라는 동일한 논리적 단위로 볼 수 있게 해줌
  - Operation
    - create, read, write, reposition (lseek), delete, open, close 등



- File Attribute (혹은 파일의 metadata)
  - 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들
    - 파일 이름, 유형, 저장된 위치, 파일 사이즈
    - 접근 권한 (읽기/쓰기/실행), 시간 (생성/변경/사용), 소유자 등



- File System
  - 운영체제에서 파일을 관리하는 부분
  - 파일 및 파일의 메타데이터, 디렉토리 정보등을 관리
  - 파일의 저장 방법 결정
  - 파일 보호 등





## Directory and Logical Disk

- Directory
  - 파일의 메타 데이터 중 일부를 보관하고 있는 일종의 특별한 파일
  - 그 디렉토리에 속한 파일 이름 및 파일 attribute들
  - operation
    - search for a file, create a file, delete a file
    - list a directory, rename a file, traverse the file system



- Partition (=Logical Disk)
  - 하나의 (물리적) 디스크 안에 여러 파티션을 두는게 일반적
  - 여러 개의 물리적인 디스크를 하나의 파티션으로 구성하기도 함
  - (물리적) 디스크를 파티션으로 구성한 뒤 각각의 파티션에 file system을 깔거나 swapping등 다른 용도로 사용할 수 있음





## Open()

![Open](https://media.vlpt.us/images/injoon2019/post/2e5a4a44-9c7c-47fd-a815-a5c92b78308e/image.png)

- open을 하면 file에 대한 metadata가 메모리로 올라감



- open("/a/b/c")

  - 디스크로부터 파일 c의 메타데이터를 메모리로 가지고 옴

  - 이를 위하여 directory path를 search

    - 루트 디렉토리 "/"를 open하고 그 안에서 파일 "a"의 위치 획득
    - 파일 "a"를 open한 후 read하여 그 안에서 파일 "b"의 위치 획득
    - 파일 "b"를 open한 후 read하여 그 안에서 파일 "c"의 위치 획득
    - 파일 "c"를 open한다.

    

  - Directory path의 serach에 너무 많은 시간 소요

    - open을 read/write와 별도로 두는 이유
    - 한번 open한 파일은 read/write 시 directory search 불필요

    

  - Open file table

    - 현재 open된 파일들의 메타 데이터 보관소(in memory)

    - 디스크의 메타 데이터보다 몇 가지 정보가 추가

      - open한 프로세스의 수
      - File offset : 파일 어느 위치 접근 중인지 표시 (별도의 테이블 필요)

      

  - File descriptor(file handle, file control block)

    - open file table에 대한 위치 정보(프로세스 별)





![open2](https://media.vlpt.us/images/injoon2019/post/6c14843c-aec8-407e-99e4-66d4ba445657/image.png)

1. 프로세스에서 open("/a/b") 명령어 입력 (system call)

2. 디스크에서 root의 metadata를 메모리에 올림 (Open file table에)

   2-1 root의 content에서 a의 metadata를 가져와서 메모리에 올림

   2-2 a의 content에서 b의 metatdata를 가져와서 메모리에 올림

   2-3 비로소 b의 content를 메모리에 올림

3. PCB에 b의 정보가 어딨는지 저장함 (descriptor)





## File Protection

![file protection](https://media.vlpt.us/images/injoon2019/post/5515d08b-5ae1-4613-8688-11af03a864d5/image.png)





## File System의 Mounting

![mounting](https://media.vlpt.us/images/injoon2019/post/913c93ee-02aa-47d6-a459-1d8332e20d78/image.png)

- 서로 다른 파티션에 있는 데이터에 접근





## Access Methods

- 시스템이 제공하는 파일 정보의 접근 방식

  - 순차 접근(sequential access)

    - 카세트 테이프를 사용하는 방식처럼 접근
    - 읽거나 쓰면 offset은 자동적으로 증가

    

  - 직접 접근(direct access, random access)

    - LP 레코드 판과 같이 접근하도록 함
    - 파일을 구성하는 레코드의 임의의 순서로 접근할 수 있음