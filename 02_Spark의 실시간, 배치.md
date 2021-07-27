# Big Data

## 3V

1. Volume
2. Velocity
  - Batch
  - Real-time
3. Variety
  - Structured
  - Unstructured
  - Semi-Structured

> 단순히 크기만해서 빅데이터가 아니라 다양한 고려 필요.

<br>

## Big Data Processing

1. Batch Processing
: 데이터는 매우 크고 복잡. **Latency를 가져도 되는 경우**
  - 큰 덩어리의 데이터 처리
  - 크고 복잡
  - 높은 latency(ex) MR)

2. Stream Processing
: 데이터가 즉각적으로 처리될 필요가 있음. 간단, 독립적일 필요가 있다.(Outlier처리, trigger 처리 등의 측면에서)
  - 한번에 처리
  - 간단하고 독립적
  - subsecond latency

3. Micro-batching(Batch와 Streaming의 중간)
  - Small batch size
  - Latency in seconds (Latency가 있으나, 1~수초안에 해결.)
  - Windowing, Stageful => Spark streaming

![image](https://user-images.githubusercontent.com/43158502/127141795-f8aac1be-33eb-4f2d-bb16-8f6a5b0c3ef4.png)
![image](https://user-images.githubusercontent.com/43158502/127141817-7360df6a-7984-4eba-b540-116acf223a8e.png)
![image](https://user-images.githubusercontent.com/43158502/127141832-012273b6-0824-42c9-9b93-90769c10de9e.png)

<br>

### Source Operator -> Processing Operator -> Sink

- Source Operator : 수집 단계 (ex) Kafka)
- Processing Operator : 처리 (ex) Spark)
- Sink : 적재 (ex) Elastic Search, Hadoop 등)

<br>

### Checkpointing
- In-memory면 연산 오류 시 재연산을 수행할 시 어떻게 하는가?
> 각각 단계가 수행되면서 문제가 생길 시 checkpoint를 통해 저장된 상태를 이용하여 재연산을 수행한다.
>
> but, checkpoint의 용량이 크면 이 시간이 오래 걸린다 => 만약 10초가 걸린다고 가정하자, 10초 data가 상당히 크다. time interval을 조정함으로써 이런 점들을 해결할 수 있다.

## Accumulator & Broadcast -> RDD 공통.
- RDD(Resillient Distributed Dataset)
> 병렬적으로 수행될 수 있는 엘리먼트의 컬렉션 

#### Accumulator : 디버깅 용으로 주로 활용, Cluster 전체 증분 가능 data

#### Broadcast : Read-only로 가져오는 것 (ex) 기준 정보)
  - **JVM GC issue**가 있다.

### 구조적인 고려사항
- data가 커지면 -> 연산 비용이 증가
- Scalability(확장성) 고려 필요
- CPU/RAM 등 추가 자원 확보 고려 필요
- Fault tolerance -> Fault가 발생 시 어떻게 resume할 것인지 고려 필요
- 기타 제약 사항 : SLAs(job scheduling, reprocessing, clustering 등) 고려 필요

<br>

## Real-time data processing use cases

### Performance tuning
- JVM 모니터링
  : driver가 죽으면 hadoop이 재실행. 보통의 원인은 RAM때문에 발생
- Batch/Window size
- Level of Parallelism
- GC/Memory usage
  : 병목 발생 부분 확인 및 해결

<br>

## Kafka
  - 분산 메세징 서버
  - 각 topic들은 partition이 구성

- producer : 메시지를 보내는 곳(ex) Kafka)
- consumer group에서 Kafka와 연결해서 메시지를 받게 된다

Spark는 개발은 쉬우나, 운영이 어렵다.
  - 누락 감수하고 실시간성을 높일 지
  - 혹은 상황에 따라 다른 방법을 선택하는 것이 필요


<br>

### DataFrame
- Structured Streaming
  - event time
  - windowing
  - session
  - sources & sink

> -> High Level Streaming API 제공.(Unified, Interactive, Batch Queries)

<br>

## MapReduce
- Mapper : 시간 별 Action에 대해 Split
- Reduce : 각 Action별 count
> output sink를 바로 지원.

<br>

#### watermark
- 바로 보내주지 않고, 늦은 message 발생 시, 탈락되는 대신에 watermark를 이용해서 허용시간 내에서는 받아주도록 설정할 수 있다.




