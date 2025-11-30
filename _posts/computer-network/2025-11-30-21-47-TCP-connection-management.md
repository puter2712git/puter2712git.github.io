---
title: TCP Connection Management
date: 2025-11-30 21:47:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

## TCP Connection Management

**TCP connection**은 client와 server 사이 positive ACK mechanism 관계로 이루어집니다.

Connection은 세 단계로 이루어집니다:
- 실제 데이터 이동 전에 sender와 receiver간의 connection이 설정됨
- data 교환에서도 전송의 안정성을 보장하기 위해 data가 잘 전송됐는지를 확인
- data 교환이 끝나면 connection이 해제됨

### Establishing TCP Connection

**3-way handshake**
1. Sender는 초기 seq num을 설정하고, TCP SYN segment를 전송합니다.
2. Receiver가 TCP SYN segment를 받으면 초기 seq num을 설정하고, TCP SYNACK segment를 전송합니다.
3. Sender는 SYNACK를 받아서 server가 살아있는지를 확인합니다.
4. Sender는 SYNACK에 대한 ACK를 전송합니다. (client-to-server data가 포함될 수 있음)
5. Receiver는 ACK를 받아서 client가 살아있는지를 확인합니다.

여기서 **ISN**(Initial Sequence Number)를 무작위로 설정하는데, 이는 이유가 존재합니다. <br>
만약 ISN을 예측 가능한 값으로 설정하게 되면, attacker는 이 연결에서 host를 사칭해서 연결을 위조할 수 있습니다.

---

이 connection을 생성하는 방식에서 발생하는 attack 기법이 또 존재합니다.

**Denial-of-service** (DoS) attack:
- TCP 서버는 각 SYN 요청마다 resource를 할당하지만, 이 resource는 제한되어 있음
- attacker는 SYN segment를 짧은 시간 내에 매우 많이 전송하지만, ACK와 SYNACK는 따로 보내지 않음
- 결과적으로 모든 server의 resource를 차지해서 다른 정상적인 트래픽에 반응할 수 없게 됨

---

### Closing TCP Connection

**4-way handshake**
1. Client에서 연결을 닫는 요청을 보냅니다. (이후 데이터는 더 이상 보낼 수 없지만 받을 수 있음)
2. Server는 이 요청에 대한 ACK를 보냅니다. (여전히 server는 데이터를 보낼 수 있음)
3. Server가 데이터를 모두 전송하면 연결을 종료하겠다는 의미의 segment를 보냅니다.
4. Client는 server가 연결을 종료하겠다는 segment를 받고 이에 대한 ACK를 보냅니다.
5. Client는 이후 곧바로 종료하지 않고 $2 \times MSL$ 시간 동안 대기합니다.
6. 해당 시간 동안 대기가 완료되면 종료합니다.

---

**Maximum Segment Lifetime** (MSL)은 TCP segment가 인터넷 시스템 내에 존재할 수 있는 최대 시간입니다. (RFC 793에서는 2분으로 정의. 30초, 1분, 2분이 적합)

$2 \times MSL$만큼 대기하는 이유:
- Server에서 FIN으로 LAST_ACK를 보냈고 client가 이를 받은 상황입니다.
- 이때 만약 client가 보낸 ACK가 손실날 경우 client에서는 연결이 종료되지만 server에서는 종료되지 않을 수 있습니다.
- 따라서 segment가 존재할 수 있는 시간인 MSL의 두 배의 시간만큼 client는 대기하게 되어 혹시 손실되는 segment가 있는지 남아서 확인하게 됩니다.