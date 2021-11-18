- [SQL 개요](#sql---)
  * [SQL 구성요소](#sql-구성요소)
  * [데이터 타입](#데이터-타입)
  * [NULL](#null)
  * [ORDER BY](#order-by)
  * [집단 함수](#집단-함수)
  * [그룹화](#그룹화)
  * [HAVING](#having)
  * [JOIN](#join)



# SQL 개요

- SQL은 현재 DBMS 시장에서 관계 DBMS(RDBMS)가 압도적인 우위를 차지하는데 중요한 요인의 하나
- SQL은 IBM 연구소에서 1974년에 System R이라는 관게 DBMS 시제품을 연구할 때 관계 대수와 관계 해석을 기반으로, 집단 함수, 그룹화, 갱신 연산 등을 추가하여 개발된 언어
- 1986년에 ANSI에서 SQL 표준을 채택함으로써 SQL이 널리 사용되는데 기여
- 다양한 상용 관계 DBMS마다 지원하는 SQL 기능에 다소 차이가 있음





- SQL은 비절차적 언어(선언적 언어)이므로 사용자는 자신이 원하는 바(what)만 명시하며, 원하는 것을 처리하는 방법(how)은 명시할 수 없음
- 관계 DBMS는 사용자가 입력한 SQL문을 번역하여 사용자가 요구한 데이터를 찾는데 필요한 모든 과정을 담당
- 자연어에 가까운 구문을 사용하여 질의를 표현할 수 있음
- 두 가지 인터페이스
  - 대화식 SQL(Interactive SQL)
  - 내포된 SQL(embedded SQL)



## SQL 구성요소

![SQL 구성요소](https://player.slidesplayer.org/90/14426990/slides/slide_46.jpg)



![명령어](https://media.vlpt.us/images/joygoround/post/a1e6928a-ce59-4869-b8ae-1e9e9a524b6f/image.png)







## 데이터 타입

![데이터 타입](https://player.slidesplayer.org/90/14426990/slides/slide_51.jpg)





## NULL

![null 1](https://player.slidesplayer.org/90/14426990/slides/slide_75.jpg)

![null 2](https://player.slidesplayer.org/90/14426990/slides/slide_76.jpg)







## ORDER BY

![order by](https://player.slidesplayer.org/90/14426990/slides/slide_79.jpg)



## 집단 함수

![집단 함수](https://player.slidesplayer.org/90/14426990/slides/slide_81.jpg)

- COUNT
- SUM
- AVG
- MAX
- MIN





## 그룹화

![그룹화](https://player.slidesplayer.org/90/14426990/slides/slide_84.jpg)



## HAVING

![having](https://player.slidesplayer.org/90/14426990/slides/slide_87.jpg)





## JOIN

![join](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/98/1861.png)

- INNER JOIN : 교집합
- LEFT JOIN : 부분집합
- RIGHT JOIN : 부분집합
- FULL OUTER JOIN : 합집합