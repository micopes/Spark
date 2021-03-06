# DAG(Directed Acyclic Graph)
![image](https://user-images.githubusercontent.com/43158502/127149886-daec9c66-a712-498a-8830-0437fde033d2.png)

- node는 **RDDs**
- arrow는 **Transformation**

> C에서 문제가 생기면 B에서부터 다시 동작.

![image](https://user-images.githubusercontent.com/43158502/127149959-3d9dba9d-b40f-44c8-940a-8c5d128662f8.png)

> n개의 stage로 나뉘고 각 stage에서 m개의 task로 나뉜다.



## Transformation
- narrow : map
- wide : sort (key를 기준으로 grouping하는 것.)
  - **network I/O 발생.**

### Action
  1. workflow의 final stage
  2. DAG가 trigger


<br>

### Partition 방법 2가지.
1. coalesce : shuffle X
2. Repartition : sort(Shuffle) 후.
> Partition은 block 단위보다 작은 small file issue와 같은 것들을 다루고자 할 때.

### Structured API

![image](https://user-images.githubusercontent.com/43158502/127150022-bc86c69d-296b-4e3f-8ab1-ef537063165e.png)

### Catalyst Optimizer
- **Catalyst Optimizer가 실질적으로 DAG를 최적화.**
- Logical Plan -> Physical Plan.
- SQL/DataFrame/Dataset 관련 없이 실행계획은 같다.


pivot : reshaping data

### Creating Rows 
- row, column 추가/제거 가능. 변경 또한 가능.
- DataFrame으로 명확하지 않은 경우 동일하게 쿼리 치고 explain 비교해보면 된다.

### Spark SQL/Hive

#### SQL
- EDA 및 분석 환경에서 데이터를 빨리 보여줘야 하는 경우 spark SQL 활용 가능 -> Zeppeline에서 자체적으로 표, 차트 확인 가능하므로.
- 모든 프로세스를 진행할 수 있으나, 그만큼의 자원이 필요하다.

### Hive
- ETL 진행 시에는 모두 Hive로.

<br>

## DataFrame/Dataset/SQL

![image](https://user-images.githubusercontent.com/43158502/127150240-24d97e41-a84c-4a99-a62a-4665704be53f.png)


- DataFrame, Dataset 사용 시 compile time 시에 미리 검출 가능.
- **DataFrame, Dataset, SQL 모두 Catalyst Optimizer를 사용하므로 성능은 거의 동일**

DataFrame은 Dataset의 subset. DataFrame은 scala의 다양한 기능을 사용할 수 있는 단위.

### 정리
- RDD는 거의 사용x
- DataFrame/Dataset(JVM을 이용하는 경우)을 주로 사용한다.
