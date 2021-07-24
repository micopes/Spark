# 실습

# Docker를 통해 이미지 설치 후 Zeppeline에서 GUI 기반 사용.
- [https://www.youtube.com/watch?v=8oJwc3uPMvo] 참고

### 사용법
- `%spark` 기본은 scala 기반 spark.
- 인터프리터를 바꾸려면 `%`를 붙여야 한다.(`%sh`, `%hive` 등과 같이 여러 '%'를 이용하여 여러 인터프리터로 변경할 수 있다.)

<br>

## RDD(Resillient Distributed Dataset)
- 데이터 컨테이너.
- Spark의 Core. RDD 기준으로 Share.

## Transformation
- Fault Tolerant
- 특정 node에서 해당 job을 수행할 수 없는 경우가 은근히 많다.
  - 중간 산출물을 다시 가져와서 재동작시킨다. (transformation 단계로 돌아가 retry. 여러 번 필요할 때에는 cache를 사용하는 것을 고려)

### RDD 프로그래밍 시 
- Python과 Spark의 속도차이가 많이 난다.
- 기능적인 면에서는 python에서도 대부분의 기능이 존재.
  - but, 속도 측면에서 scala가 유리.

### Scala
- JVM위에서 동작되는 functional programming language
- overhead를 줄이고 성능을 높이고자 할때.
- **analysis에는 python을 이용해서 spark를 사용하면 되지만, engineering시 scala를 이용하는 것이 필요**

<br>

## Operation

### Transformation
  - Lazy operation이라고도 한다. **실질적으로 spark이 일을 하는 단계는 아니다.**
  - 새로운 RDD를 생성한다고 보면 됨.
  - ex) map, filter, groupBy, join

### Action
  - 값을 반환하거나, 저장소에 쓰는 등의 실질적인 동작을 하는 단계.
  - Driver쪽으로 결과를 리턴하거나 저장하는 동작.
  - ex) count, collect, save

### DAG 
> **Transformation**을 여러 번 하더라도 실질적으로는 **Action** 단계에서 최적화.

## RDDs vs DataFrame

### RDD
- Low-level interface into spark

### DataFrame
- DataFrame을 사용하는 경우 spark와 python의 성능 차이가 거의 발생하지 않는다.
- schema를 가진다.
- cached, optimized

> RDD보다 DataFrame이 좀 더 직관적이다.


<br>
Spark는 메모리뿐만아니라 CPU도 중요.

### Broadcast vs Accumulator

- Broadcast
  - Webside join
  - Driver에서 각 executor들로 data를 load해서 가지고 있는 것.
- Accumulator
  - 디버깅 용도
  - Driver에서 event를 받아서 수정된 것들을 들고 있는 것.

> Broadcast와 Accumulator는 모두 **long-learning process이므로 메모리 관리에 유의!**