---
title: TCP Congestion Control
date: 2025-11-30 22:14:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

## TCP Congestion

**Datagram packet switching network**에서는 다음과 같은 특징이 존재합니다:
- 데이터는 여러 개의 packet으로 나뉨
- buffer를 이용한 store & forward
- packet forwarding을 위한 link 공유

이때, 너무 많은 packet이 router에 도착해서 처리하기 힘들어질 때 **congestion**이 발생합니다. <br>
Router의 버퍼가 꽉 차게 돼서 결국 overflow되어 packet이 손실됩니다.

---

### Effects of Congestion - Delay

**Delay**는 다음과 같이 구성됩니다.

$$
\sum \{(\text{queuing delay} + \text{processing delay} + \text{propagation delay}) \text{ at each node} \}
$$ <br>
- **processing delay**: depends on packet size
- **propagation delay**: depends on link bandwidth
- **queuing delay**: traffic load가 증가할수록 exponentially 증가

Congestion control이 없는 환경:
- traffic이 증가 $\to$ congestion 발생 $\to$ queue delay 증가 $\to$ e2e delay 증가

---

### Effects of Congestion - Throughput

**Network throughput**이란 packet이 network를 통해 성공적으로 전송되는 속도를 의미합니다.

$$
throughput = {\text{total amount of successfully received packets} \over \text{total transmission time}} \text{(in bits/sec)}
$$

Traffic load가 조금 증가할수록 throughput은 선형적으로 증가하게 됩니다. <br>
하지만 load가 계속해서 증가하면 throughput의 증가율이 점점 떨어지다가 결국 overflow에 의해 packet이 버려지는 순간까지 오면 throughput은 drastically 감소하게 됩니다.

---

## Congestion Control

**Congestion Collapse**는 다음과 같은 상황을 의미합니다:
- traffic 증가 $\to$ buffer overflow $\to$ timeout $\to$ retransmission 증가 <br>
$\to$ congestion amplification $\to$ more timeout $\to$ more retransmiossion <br>
$\to$ congestion amplification much more $\to$ $\cdots$ $\to$ **congestion collapse**

**Congestion Avoidance**란 congestion이 발생하기 전에 이를 제어해서 congestion을 회피하는 기법입니다. <br>
이전에 throughput이 drastically 감소되는 부분보다 traffic 양을 낮게 유지하는 것을 목표로 합니다.

---

**Explicit Congestion Control**:
- 네트워크 요소들을 통해 congestion information을 알아차리고, 이를 sender에게 직접 알려줍니다.
- 라우터와 같은 네트워크 장치가 이러한 기능을 가지고 있어야 합니다.

**Implicit Congestion Control**:
- 네트워크 장치로부터 명시적으로 피드백을 받지 않습니다.
- end system에서 loss, delay와 같은 네트워크 상태를 통해 congestion을 추론합니다.
- TCP가 채택한 방식입니다.

---

## TCP Congestion Control

**TCP Congestion Control**:
- sender-based
- **congestion window** (cwnd)
    - RTT, timeout 등을 monitoring하여 congestion을 추정
- sender는 자신의 window size가 cwnd를 넘지 않도록 제한합니다.

---

TCP에는 두 가지의 window가 존재합니다:
- **rwnd**(receive window)
    - TCP flow control에 사용
    - receiver buffer의 overflow를 방지
    - receiver는 receiver buffer의 남은 크기를 알림
- **cwnd**(congestion window)
    - TCP congestion control에 사용
    - router buffer의 overflow를 방지
    - sender에 의해 크기가 제한됨

네트워크 상황에 의해 두 window 모두 크기가 동적으로 변하게 됩니다. <br>
결국 최종적으로 TCP window의 크기는 다음과 같이 설정됩니다.

$$
\text{TCP window size at sender} \leq \text{min}(rwnd, cwnd)
$$

---

### AIMD

**AIMD**(Additive Increase Multiplicative Decrease):
- 가장 기반이 되는 TCP congestion control methods
- congestion 상태를 감지하고 이에 따라 `cwnd`를 dynamic하게 변화시킴
    - congestion이 감지되지 않으면 `cwnd`는 linearly 증가: **AI**(Additive Increase)
    - congestion이 감지되면 `cwnd`는 multiple 감소: **MD**(Multiplicative Decrease)

알고리즘: <br>
$$
\text{cwnd}(n) \text{: congestion window size at } n \text{-th event (ACK or loss)}
$$
$$
\alpha \text{: additive increase parameter } (\alpha \gt 0)
$$
$$
\beta \text{: multiplicative decrease parameter } (0 \lt \beta \lt 1)
$$

$$
cwnd(n + 1) = \begin{cases}
cwnd(n) + \alpha \text{, if congestion is not detected (for a normal ACK)} \\
cwnd(n) \times \beta \text{, if congestion is detected (for a timeout event)}
\end{cases}
$$

- $\alpha$는 RTT 당 1 MSS 만큼 `cwnd`가 증가하도록 설정
- $\beta$는 일반적으로 $1 \over 2$로 설정됩니다.

---

AIMD example:
- `cwnd` 1로 시작
- ACK 받음: $inc = 1/1$이므로 $cwnd = 1 + 1 = 2$로 설정
- ACK 받음: $inc = 1 / cwnd = 1 / 2$이므로 $cwnd = 1 + 1/2 = 3/2$로 설정

각 ACK마다 $increment = 1 / cwnd$로 설정되고, 다음의 `cwnd`는 $cwnd = cwnd + increment$로 설정됩니다.

---

AIMD는 간단하다는 장점이 있지만, linear additive 방식은 network capacity까지 도달하는 데 너무 오래 걸린다는 단점이 존재합니다.

---

### Slow Start

AIMD는 linear additive increase 방식이기 때문에 연결 초기 단계에서부터 네트워크 용량을 채우는 데까지 도달하는 시간이 너무 오래 걸리는 단점이 존재합니다.

TCP Tahoe부터는 **Slow Start** mechanism이 도입되었고, 이는 초기에 `cwnd`에게 exponential increase를 제공합니다.

---

**Slow Start**는 first loss event가 발생할 때까지 `cwnd`를 exponentially 증가시킵니다.
- 1 MSS에서 시작
- RTT마다 double

알고리즘: <br>
- initially $\textrm{cwnd} = 1 \text{ MSS}$
- ACK가 도착할 때마다 `cwnd`는 다음과 같이 증가
    - $\textrm{increment} = 1$
    - $\textrm{cwnd} = \textrm{cwnd} + \textrm{increment}$
- segment loss가 발생할 경우 (by timeout or 3 duplicate ACKs)
    - $\textrm{cwnd} = 1$

---

Slow Start Example:
- `cwnd` 1로 시작
- ACK 받음: $cwnd = 1 + 1 = 2$
- ACK 받음: $cwnd = 2 + 1 = 3$
- ACK 받음: $cwnd = 3 + 1 = 4$ <br>
...

결국 `cwnd`가 증가하면서 `cwnd` 만큼 segment를 보내기 때문에 RTT마다 `cwnd`가 두 배씩 증가하게 됩니다.

---

## Congestion Avoidance (Tahoe)

앞서 본 Slow Start는 실제로 *slow*하지 않습니다. <br>
`cwnd`는 RTT마다 두 배로 증가하기 때문에 congestion이 발생할 확률이 너무 빠르게 높아집니다.

**Congestion Avoidance**는 `cwnd`의 exponential 증가를 제한합니다.

---

**Slow Start Threshold**(ssthresh)는 **SS**(Slow Start)를 사용할지 **CA**(Congestion Avoidance)를 사용할지를 결정합니다. <br>

- 각 ACK마다:
    ```
    if (cwnd < ssthresh)
        cwnd += 1;          // slow start (SS)
    else
        cwnd += 1 / cwnd;   // congestion avoidance (CA)
    endif
    ```
- loss (indicated by timeout) or 3 duplicate ACKs:
    ```
    retransmit all unacked;     // GBN ARQ
    ssthresh = cwnd / 2;
    cwnd = 1;                   // slow start
    ```

---

## Fast Recovery (Reno)

Lost segment는 **timeout**이나 **3 duplicate ACKs**로 감지됩니다.

**Timeout**은 **congestion possibility**가 매우 높음을 나타냅니다. <br>
**3 duplicate ACKs**는 **congestion possibility**가 매우 낮거나 일시적인 congestion임을 나타냅니다. (fast retransmit)

Moderate 또는 instantaneous congestion의 경우 1 MSS부터 다시 Slow Start를 적용하게 되면 bandwidth를 낭비하게 됩니다.

---

**Fast Recovery**에서는 ACK를 받을 때는 Tahoe의 경우와 같습니다.

Congestion이 감지되면 해당 event가 **timeout**인지 **3 duplicate ACKs**인지에 따라 operation이 달라집니다.
- for loss (indicated by timeout)
    ```
    retransmit 1st unacked; // unlike GBN ARQ
    ssthresh = cwnd / 2;
    cwnd = 1;               // slow start
    ```
- for 3 duplicate ACKs
    ```
    retransmit 1st unacked; // unlike GBN ARQ
    ssthresh = cwnd / 2;
    cwnd = cwnd / 2 + 3;    // fast recovery
    ```

---

## TCP Fairness

**Fairness**의 목표는 다음과 같습니다:
- $K$개의 TCP session에 대해 bandwidth R의 같은 bottleneck link를 공유
- 평균값인 $R / K$으로 같은 rate를 가지도록 함