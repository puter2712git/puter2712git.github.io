---
layout: post
title: 2. Overview of Parallel Computing
categories: [memoing]
tags: [Distributed Parallel Computing]
---

전통적으로 소프트웨어는 직렬 컴퓨팅 방식으로 작성되었다(serial computing). Problem을 해결하기 위해 instruction sequence를 순차적으로 실행한다. 특정 time에 하나의 instruction만 실행되는 방식이다. <br>

반면 parallel computing은 problem을 여러 개의 부분으로 나누어 이를 concurrently 해결되도록 만든다. 각 부분의 instruction은 다른 processor에서 동시에 실행된다.
<br>

컴파일러는 instruction-level parallelism을 지원하지만, 이는 한계가 존재한다. 따라서 우리는 병렬적으로 생각할 줄 알아야 한다. <br>

Serial program을 parallel하게 실행하는 방법에는 여러 가지가 존재한다. <br>
- Serial program을 재작성
- Serial program을 parallel program으로 변환해 주는 프로그램을 작성
    - 매우 어려움
    - 한계 존재함
- 결국 처음으로 돌아가서 병렬적으로 알고리즘을 재작성하는 것이 최고의 솔루션일 때가 있음

<br>

Work을 나누는 건 크게 두 종류가 있다.

### Data Parallelism
```c
sum = 0;
for (i = 0; i < n; i++) {
    x = Compute_next_value(. . .);
    sum += x;
}
```

Problem을 해결하기 위한 데이터를 partitioning한다.

### Task Parallelism
```c
if (master core) {
    sum = my_x;
    for each core other than myself {
        receive value from core;
        sum += value;
    }
} else {
    send my_x to the master;
}
```

Problem을 해결하기 위한 task를 partitioning한다.

<br>

그렇다면 더 나은 Parallel Algorithm을 설계하려면 어떻게 해야 할까?
- Master core가 모든 일을 하지 않게 하기
- 다른 core와 공유하기
- Core끼리 짝을 만들기 (0번 core는 1번 core의 결과를 더하고, 2번 core는 3번 core의 결과를 더하고...)
    - 짝수 번째의 core와 홀수 번째의 core끼리 짝을 만들기

<br>

Parallel Computing에서는 **concurrency**와 **parallel** 두 용어가 함께 나오는데, 둘의 차이점을 확인해보자. <br>
- **Concurrency**: 여러 작업이 동일한 시간 간격 동안 진행됨, 단일 CPU가 프로그램을 빠르게 전환해가며 모두 동시에 진행되는 것처럼 보이는 것이 예시이다.
- **Parallelism**: 여러 작업이 다수의 처리 장치에서 동시에 실행되는 것이다. speed와 throughput이 향상된다.