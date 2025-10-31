---
layout: post
title: 1. Why Parallel and Distributed?
categories: [memoing]
tags: [Distributed Parallel Computing]
---

대규모 데이터 분석, AI의 급속 발전으로 high-performance computing이 빠르게 요구되었다. <br>
**CUDA**와 같은 빠른 연산에 최적화된 방식도 존재하지만, 이런 방법은 single-node에 국한된 방식이고, one layer의 병렬 처리 방식이다.
따라서 우리는 **MPI**, **Spark**를 통해 병렬 분산 컴퓨팅을 이해하고 portability & scale 측면에 관심을 기울일 것이다.

> CUDA: NVDIA-specific GPU programming

<br>

병렬 컴퓨팅과 분산 컴퓨팅은 비슷해 보이지만 다르다. 가장 큰 차이점으로는 협력하는 프로세서끼리의 거리라고 말할 수 있다. <br>
- 병렬 컴퓨팅: same box(shared memory)
- 분산 컴퓨팅: across the network(message passing) <br>

다시 구분하면, parallel computing은 **performance-oriented**하고 distributed computing은 **scale/fault-tolerance-oriented**(**cluster-based parallelism**)이라고 말할 수 있다.

<br>

병렬은 우리에게 속도 이점을 준다. 분산은 reliability와 scalability를 준다. 현대 workload는 이 둘 모두를 필요로 한다. 아래는 왜 현대 workload가 분산 컴퓨팅 또한 필요로 하는지, 분산 컴퓨팅을 도입할 때 주의할 점을 정리한 내용이다. <br>
- Capacity Limits: 제한된 용량으로 분산 시스템은 피할 수 없다.
- Data Movement Cost: 대규모 환경에서는 network와 storage가 bottleneck이 될 수 있다. 따라서 data locality를 통해 성능을 향상시킬 수 있다.
- High Availability & Fault Tolerance: cluster 규모에서는 single-node failure는 일상이기 때문에, replication, checkpoint/restart, retries 메커니즘이 필요하다.
- Scalability & Elasticity: workload는 fluctuate(변동)하므로, 스레드 수준을 넘은 병렬 처리를 해야 한다. 따라서 scale-out & orchestration 등을 수행하는 도구가 필수적이다.
- Heterogeneity & Multi-tenancy: CPU, GPU 등의 다양한 하드웨어가 하나의 클러스터를 공유하기 때문에, resource management와 scheduling은 efficiency와 fairness를 결정하는 핵심 요소이다.
- Pipelines & Streaming: 현대의 workflow는 *data aggregation -> ETL -> training/inference -> serving*과 같은 파이프라인이기 때문에 buffering(Kafka), backpressure(Flint), SLA와 같은 시스템 수준의 기능을 고려해야 한다.
- Global Synchronization Costs: 코어 수가 많으면 전역 동기화 메커니즘(barrier, lock)의 비용이 매우 커진다. 이 때문에 domain decomposition, async coordination, collectives와 같은 기술이 필요하다.