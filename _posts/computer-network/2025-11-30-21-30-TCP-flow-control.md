---
title: TCP Flow Control
date: 2025-11-30 21:30:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

## Flow Control

TCP에서 수신자는 **receiver buffer**를 사용합니다. 이 버퍼 상태를 통해 송신자의 전송 속도를 제어하게 되고, application이 data를 읽는 속도와 송신자의 전송 속도를 일치시키려고 합니다.

TCP flow control이 필요한 이유는 receiver buffer의 overflow를 방지하기 위함입니다. <br>
만약 송신자가 수신자의 처리 능력보다 훨씬 빠르게 데이터를 계속 보내게 되면 버퍼가 가득 차게 됩니다. 이러면 새로 도착하는 segment가 모두 drop되게 됩니다. <br>
즉, flow control을 통해 송신자는 수신자가 감당할 수 있을 속도로 segment를 전송하게 됩니다.

---

Receiver buffer의 구성:
- `RcvBuffer`: receiver buffer의 크기
- `Bdata`: buffer의 buffered data의 크기
- `Receive Window (rwnd)`: 남는 공간 (송신자가 segment를 보내면 채워짐)

$rwnd = RcvBuffer - Bdata$

수신자는 `rwnd` 값을 TCP header에 추가해서 송신자에게 알려주고, 송신자는 자신의 window 크기를 조절해서 `rwnd` 값보다 크지 않도록 만듭니다.

---

이렇게 flow control은 전적으로 receiver-based이며, 수신자는 송신자가 보내주는 `rwnd` 값으로 자신의 window size를 제한합니다.