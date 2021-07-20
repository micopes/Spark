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






