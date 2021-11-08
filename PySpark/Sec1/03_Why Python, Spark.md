# Why Python, Spark?

- 32, 64 RAM는 로컬에서 간단한 작업을 하기에는 용이하지만 `not big data`

### 분산 환경의 장점
- 분산 환경에서 **값싼 여러 분산 machine**의 작용이 하나의 비싸고 고성능의 machine보다 효율성 및 용량이 뛰어나다.
- **scale out, 수평 확장이 쉽다. (확장성이 좋다)**
- **Fault Tolerance 보장** -> 특정 machine에 결함 혹은 고장이 발생해도 정상적 혹은 부분적으로 수행 가능

## Hadoop
- 대용량 분산 처리 방식
- **HDFS**를 사용한다.
  - Block을 복제(duplicate)하여 `Fault Tolerance` 보장
- MapReduce를 통해 대용량 분산 처리 수행
  - MapReduce 시 `Job Tracker`와 `Multiple Task Tracker`로 나뉜다.
    - Job Tracker가 Task Tracker로 작업을 보낸다
    - Task Tracker는 각각 작업을 할당 및 감시한다.

## Spark
- 쉽고 빠르게 Big data를 사용할 수 있는 기술
- **Apache** Open Source Project
- **Flexible Alternative to MapReduce**(하둡의 대체재가 아닌 MapReduce의 대체재이다.)
- **다양하게 저장된 데이터의 형식에 사용 가능**
  - Cassandra
  - AWS S3
  - HDFS
  - And more.


### Spark vs MapReduce
- MapReduce는 file이 HDFS에 저장되어 있어야 하지만, spark는 다양하게 저장된 형식에 사용 가능
- Spark가 MapReduce보다 100배 빠르다.
  - Spark : **Memory Based**
  - MapReduce : **Disk Based**

## Spark RDD
- Distributed Collection of data
- Fault Tolerance
- Parallel operation - partioned
- Ability to use many data resources

![image](https://user-images.githubusercontent.com/43158502/140716041-8e5896b3-6d73-4e4c-af33-248e5ccffc5d.png)

### Transformation
  - Lazy operation이라고도 한다. **실질적으로 spark이 일을 하는 단계는 아니다.**
  - 새로운 RDD를 생성한다고 보면 됨.
  - ex) map, filter, groupBy, join

### Action
  - 값을 반환하거나, 저장소에 쓰는 등의 실질적인 동작을 하는 단계.
  - Driver쪽으로 결과를 리턴하거나 저장하는 동작.
  - ex) count, collect, save

![image](https://user-images.githubusercontent.com/43158502/127147567-ae51f802-f10f-4806-ad5c-f4bc93d0f74c.png)

### DAG 
> **Transformation**을 여러 번 하더라도 실질적으로는 **Action** 단계에서 최적화.
