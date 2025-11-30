---
title: TCP Retransmission
date: 2025-11-30 20:27:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

TCP 환경에서는 전송하려는 데이터가 손실되거나 하는 등의 오류가 발생할 수 있습니다. 이를 위해 송신자에서는 retransmission을 준비하곤 합니다. <br>

TCP retransmission을 위한 기능들:
- pipelined segments (sliding window)
- cumulative ACKs
- timeout and retransmission

앞서 말했듯 TCP는 하나의 RTT만을 계산하기 때문에 하나의 retransmission timer만 존재합니다. (the oldest unACKed segment = at the front of window)

TCP retransmission은 **timeout event**가 발생했을 때나, ACK가 duplicate하게 전송됐을 때 발생합니다. ACK가 duplicated하게 전송된 경우에는 fast retransmit으로 손실된 것으로 추정되는 segment를 곧바로 재전송합니다.

GBN ARQ와 다르게 TCP는 오직 손실된 segment만을 재전송합니다.

아래에서 몇 가지 시나리오를 살펴보겠습니다.

---

1. A가 B에게 하나의 segment를 전송
2. A가 timer를 시작하고 B로부터 ACK를 받을 때까지 wait <br>
3. B에서 A로의 ACK가 손실된 경우 <br>
3-1. timer가 만료되면 A는 동일한 segment를 전송 <br>
3-2. B가 이미 동일한 segment를 받았다면 버림

---

1. A가 두 개의 segment를 연속적으로 전송
    - A는 첫 번째 segment에 대한 timer만을 작동
    - B는 각 segment에 대해 개별적인 ACK를 전송
    - 이때 A의 timeout 이후에 두 개의 ACK가 전송됐다면
2. A가 timeout event 발생 (ACK를 받기 전)
    - A는 첫 번째 segment를 재전송
    - 이때 두 번째 segment에 대한 ACK가 새로운 timeout 이전에 도착
    - second segment는 재전송하지 않음

---

1. A가 두 개의 segment를 연속적으로 전송
2. 첫 번째 segment에 대한 ACK가 손실됨
3. timeout 이전에 A가 두 번째 segment에 대한 ACK를 받음
4. 하지만 A는 B가 전송한 두 번째 ACK에서 119 byte를 통해 모든 데이터를 잘 받았음을 알 수 있음
5. 따라서 A는 따로 재전송하지 않음

---

## ACK Generation

수신자가 ACK를 보낼 때 여러 규칙이 존재합니다.

### Delayed ACK

송신자가 segment를 순서대로 잘 전송했습니다. <br>
수신자는 이 segment들을 순서대로 잘 받았습니다. 이때 수신자는 segment를 받는 즉시 바로 ACK를 보내지 않고 다음 순서의 segment가 전송될 때까지 잠시 기다렸다가 ACK를 보내게 됩니다.

이는 여러 segment가 전송될 경우 하나의 ACK로 한번에 보내기 위해 일부러 delay를 주는 것입니다. (piggyback)

---

### Immediate and Cumulative ACK

위에서 수신자가 delayed ACK를 이용해 다음 segment를 기다리는 중, 순서에 맞게 다음 segment가 도착했다면 또 기다리는 것이 아닌 곧바로 ACK를 합쳐서 보내게 됩니다.

즉, 만약 안정적인 네트워크 환경이 있다고 할 때 $N$개의 segment를 수신자에게 보낸다고 해봅시다. 그럼 수신자는 $\lfloor N / 2 \rfloor$ 개의 ACK를 전송하게 됩니다.

---

### Immediate and Duplicate ACK

수신자가 순서에 맞지 않는 segment를 전송받아서 데이터의 gap을 감지한 상황을 살펴봅시다. 그럼 수신자는 이 gap에 해당하는, 필요한 데이터를 요청하기 위해 duplicate ACK를 곧바로 전송하게 됩니다. <br>
(필요한 것은 이전에 전송되어야 할 데이터이므로, 중복된 ACK를 전송하게 됩니다.)

---

### Immediate and Cumulative & Duplicate ACK

수신자가 데이터의 gap을 채우기 위해 전송된 데이터를 잘 받았을 때는 ACK를 즉시 전송합니다.

---

## Fast Retransmit

Timeout 기반 retransmission은 문제가 있는데, 바로 timeout period가 가끔 상대적으로 긴 경우입니다. 이는 손실된 packet을 재전송할 때 긴 delay를 유발할 수 있습니다.

**Fast retransmit**은 timeout에만 의존하지 않고 손실을 빠르게 감지하기 위해 duplicate ACK를 사용합니다. 송신자가 segment를 연속적으로 보낼 때 segment가 손실되면 많은 duplicate ACK가 발생할 것입니다. 이는 segment lost는 duplicate ACK의 발생으로도 감지할 수 있음을 의미합니다.

---

송신자가 같은 data에 대한 3개의 duplicate ACK를 받게 되면 timer가 만료되기 전이어도 손실된 segment를 재전송하게 됩니다. <br>

Q: 3개의 duplicate ACK를 기준으로 삼은 이유:
- 네트워크 환경에서는 segment가 손실되지 않더라도 순서가 뒤바뀌어 오는 경우가 존재
- 이는 2개의 duplicate ACK를 발생시킬 수 있음
- 따라서 3개 이상의 duplicate ACK가 연속적으로 도착했다는 것은 실제로 손실된 데이터가 존재할 가능성이 매우 높음을 의미