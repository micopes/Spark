## Apache

- Hadoop Common
- HDFS (Hadoop Distributed File System)
- Hadoop Yarn : Hive, Spark 등 다양한 Application을 돌리기 위한 Research management 
- Hadoop MapReduce

<br>

### HDFS 
2004년 GFS 논문에서 시작, 하둡 분산시스템을 지원하는 것을 목표로 함.

- Locality, Block이 잘 구성되어있게 되면 병렬성이 높아진다.
- 문제 생길 시 Recompute 필요.
- Reliability를 위해 HDFS에서는 copy된 것을 저장.
- Large File에 유용
  - Small File이 많으면 애를 먹을 수 있다. block 단위로 이를 합치고 따로 같은 파일 처리를 해줘야 하기 때문.
- Disk I/O에 접근하므로, Latency가 발생할 수 있다.
  - **Spark는 메모리를 이용하므로 Latency를 줄인다.**

<br>

### Master-Slave Design
- Master node(name node)
  > metadata managing : 백업, 스냅샷을 통해 고가용성(시스템이 오랫동안 지속적으로 정상 운영이 가능한 성질) 보장

- Slave node
  > 실제로 데이터를 저장하게 되는 data node.


- Name mode
  - metadata
  - name
  - location
  - directory
- Data node
  - Block 단위로 data 저장.

<br>

### HDFS

> Enterprise용 DBMS(ex) RDB)는 appliance 형태로 HW/SW가 같이 들어오지만,
> 
> Hadoop은 Commodity(쉽게 구할 수 있는 일회용품 같은 것)

- 특정 node가 *fail*이 나는 경우
  > 생각보다 많이 일어나는 경우이다(주로, disk 깨짐, OS fault 등)
  > 
  > HDFS은 block을 copy함으로써 **Fault Tolerance**하다.


#### 주요 특징
- HDFS는 block 단위로 나눠서 저장
- small file의 경우 따로 처리 필요
- 1개의 block당 1개의 node가 대응되는 것은 아니다.
  - 여러 번 복제된다.(default 설정 : 3) -> **Fault Tolerance를 보장한다.**
- **Block이 잘 분배되어 있으면 병렬성이 높아진다.**

<br>

### MapReduce

- Hadoop Ecosystem/Spark 등의 기본 패러다임.
- 전통적인 Parallel Programming(Lock, Semaphore 등)의 특징?
  - 잘못 프로그래밍되는 경우 성능 저하에 심각한 영향이 있을 수 있다.
  > 이런 문제를 방지하기 위해서 Fault Tolerance에 대한 매커니즘, 프레임워크 등을 저장하는 것.
  > 
  > 프로그래밍에 대한 이슈를 많이 고려하지 않고 비교적 간단하게 MapReduce를 적용함으로써 해결할 수 있다.

#### Word Count Example
![image](https://user-images.githubusercontent.com/43158502/126293384-af1fe8b2-7bdb-477f-8f23-d1406454360e.png)

- 위의 그림에서 Shuffle & Sort를 통하여 각 단어를 Tokenize해서 몇번씩 노출이 되었는지 count.

<br>

### Apache Spark
- UC 버클리에서 2009년에 시작.
- Fast and general purpose cluster computing system.

- Disk
  - 10배 빠르다.
- 메모리
  - 100배 빠르다.

- 머신러닝의 반복적인 알고리즘, 연산을 처리할 때 유용하다.

#### Spark가 다루는 것?
> 병렬 프로세스하는 라이브러리의 집합.

- RDD, Distributed Variable : Low-level APIs
- datasets, DataFrames, SQL : Structured APIs
- Streaming, Analytics, Ecossystem, Libraries

<br>

#### MapReduce vs Spark
![image](https://user-images.githubusercontent.com/43158502/126294341-c20b584f-515a-4e23-8bc0-d7f0814fdae5.png)
![image](https://user-images.githubusercontent.com/43158502/126294711-ed0cec33-b6e8-471c-af5c-d67b28e5c306.png)

- MapReduce
  > 1. Disk I/O 를 한다.
  > 2. 코드 스니펫을 보면 더 많은 것을 다룰 수 있다.
  > 3. 성능 및 노드 개수에서 Spark가 유리.
- Spark
  > 1. Latency를 줄이기 위해 Disk I/O 대신 Memory에서 연산.(In-Memory)
  > 2. Function Language, 코드가 간결, 추상화되어 있음 -> 생산성 측면에서 강점.
  > 3. 성능 및 노드 개수에서 Spark가 유리.

<br>

#### Apache Spark 역할
> 여러 data source를 이용하여 computing 및 분석

- 데이터 분석 지원
- 머신러닝, 그래프 분석
- 실시간 처리

#### Spark Basic Architecture
![image](https://user-images.githubusercontent.com/43158502/126295099-979f1e0a-0617-41f6-b66e-8dfcde51fddb.png)
> Cluster Manager에 대해 관리된다.

- Submit -> Driver Process가 뜬다. -> Execute하고싶은 것들 제출 -> 할당받은 것들로 작동

#### Spark Language APIs
![image](https://user-images.githubusercontent.com/43158502/126295495-b80441de-14ba-49cb-a4a4-fdbcf97e0fcd.png)

- 다양한 언어를 지원.


#### Spark에 대한 오해.
- Spark이 모든 것의 bestfit은 아니다. 기존의 Hadoop, NoSQL 등과 결합하여야 더 유용하게 사용할 수 있다.
- DB 대체가 아니다. 속도가 DB를 사용하는 것보다 안나온다. 데이터 분석가, 엔지니어의 관점에 따라 차이가 있을 수 있다.(기능은 할 수 있지만, 연산 속도 측면에서 엔지니어는 아니라고 생각하는 듯.)
- **MapReduce의 확장이 Spark. 메모리 연산에서의 강점.**

### 구성 예시
- 실시간 수집(Kafka)
- Spark Streaming(Spark) -> Hadoop에 적재
- Dashboard(Elastic Search) : 실시간 Dashboard로 확인 가능.



## 결론.
- Spark는 Hadoop의 MapReduce를 대체할 수 있다. (Memory I/O이므로 Latency를 줄여서 더 빠르다.)
- 대체 기술이 많이 있으나, 가장 빠르게 시도할 수 있는 방법이다.


