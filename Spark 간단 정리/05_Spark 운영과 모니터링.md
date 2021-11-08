# 운영과 모니터링
- 주된 성능 이슈?
  - Out-of-memory 발생.
    - ETL/분석 등 주기가 있는 것은 거의 영향 X
    - **long learning process 시 주의. GC 필요**

> Application -> Jobs -> Stages -> Tasks

- Cluster는 몇만 단위의 task로 이루어짐.
- Task는 core의 개수와 관련이 있다.
- skewed된 data는 따로 처리할 필요가 있다.
- 단일 node가 아니라 여러 분산 node가 처리되어야 하므로 cluster manager에서 이런 자원(cluster worker)들을 할당
  - resource가 없을 시 다시 할당 필요.

<br>

## Cluster 안에/밖에 Driver가 있는 것의 차이

- Cluster 안에 Driver : 대다수의 경우
  - cluster manager가 문제 상황 발생 시 처리 가능.

- Cluster 밖에 Driver : Driver가 Heavy하거나 다른 이유에 의해


## 여러 용어
- Driver : Spark Content(main 함수)를 포함하는 프로세스 
  - 실제로 동작하는 application
- Executor : Spark의 task들이 parallel하게 동작될 수 있도록 각각의 node에 분산되서 동작할 수 있는 프로세스
  - Application에 대한 executor 정보.
- Master : Application을 manage할 수 있는 프로세스
  - 실질적인 동작하는 cluster의 master worker
- Worker : Executor들의 node 단위
  - 실제로 task들이 수행되는 곳

## Spark 구성 옵션

- Local mode

![image](https://user-images.githubusercontent.com/43158502/127152560-4f42ae61-b084-4bbc-9e1a-6a1cc73753a1.png)

- Standalone mode

![image](https://user-images.githubusercontent.com/43158502/127152600-b66d14b7-4951-4cef-a794-6bebb10d3610.png)

- Yarn mode

![image](https://user-images.githubusercontent.com/43158502/127152631-00f4b755-4da2-4faa-9446-34bf1d64476f.png)

  - Yarn에게 resource 요청
  - Yarn의 Resource manager가 이를 할당
  - 자원이 할당되면 container에 이러한 것들이 할당

## Monitoring 해야하는 부분들!
- Cluster
  - Executor
  - Driver
- **JVM**
- Spark Applications and Jobs
- OS/Machine

> Spark UI에서 이런 기본적인 요소들 monitoring 가능.

