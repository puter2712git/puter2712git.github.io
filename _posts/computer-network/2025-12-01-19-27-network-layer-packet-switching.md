---
title: Network Layer - Packet Switching
date: 2025-12-01 19:27:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

이전에 **Circuit Switching** 방식을 공부했습니다. Circuit switching은 connection-oriented 방식으로, 특정 통화를 위해 resource가 dedicated하게 할당됩니다. 따라서 만약 data connection이 idle한 경우 resource는 그대로 낭비되게 됩니다. 또한 음성 트래픽은 전송 속도가 일정해야 하기 때문에 data rate가 고정되어 있습니다.

---

## Packet Switching

**Packet Switching**에서 data는 작은 packet으로 전송됩니다. (일반적으로 1000 octets) <br>
각 packet은 user data와 control information을 가지고 있습니다.

Router는 buffer를 이용해 packet을 store하고 forward합니다. <br>
Packet은 received, stored(buffered), 그리고 다음 node로 passed on 됩니다.

모든 resource는 모든 user에게 공유됩니다. (router, link와 같은 resource를 의미)

---

Packet은 두 가지 방식으로 처리됩니다:
- **virtual circuit**: packet이 전송되기 전 preplanned route를 설정함
- **datagram**: 이전 packet의 reference 없이 각 packet은 독립적으로 처리됨

**TCP/IP Protocol Suite**의 Network Layer에서는 **Datagram Packet Switching**이 채택되었습니다.

---

### Virtual Circuit Packet Switching

**VC Packet Switching**은 packet이 preplanned route로 전송되는 방식입니다.
- 연결 경로의 설정은 packet을 전송하기 전에 설정되어야 합니다. (dedicated path는 아님)
- 각 packet은 목적지의 주소 대신 **Virtual Circuit Identifier**(VCI)를 보유하고 있습니다.
- 이 방식은 packet의 순서가 뒤바뀌는 일이 없고, 손실도 없습니다.

---

**VC**는 다음과 같이 구성됩니다:
- source to destination의 경로
- 경로의 link **VCIs**(Virtual Circuit Identifier) 
- 경로의 router에 있는 forwarding table의 entries

**VC forwarding table**:
- VC에 속하는 packet으로 forwarding (destination address가 아님)
- 각 link에서 VCI는 바뀔 수 있음
- 새로운 VCI가 forwarding table로부터 들어옴

---

**VC Packet Switching**에서는 packet 교환 전에 경로가 설정되어야 합니다.

**VC Signaling protocol**은 이 가상 회선을 관리하고 설정합니다.
- VC를 setup, maintain, teardown합니다.
- VC router는 연결 상태를 유지합니다.
- 예전 전통적인 패킷 교환 방식의 네트워크에서 사용되었습니다. (ATM, frame-relay, X.25)
- 현대 인터넷에서는 사용하지 않습니다.

---

### Datagram Packet Switching

**Datagram Packet Switching**에서는 각 packet이 독립적으로 처리됩니다.
- Packet은 어떤 경로로도 이동할 수 있습니다.
- 각 packet에 대한 routing decision이 필요합니다.

Packet 전송은 **connectionless**합니다.
- packet의 순서가 뒤바뀌어 전달될 수 있습니다.
- packet이 손실될 수 있습니다.
- unreliable

Receiver는 packet을 재정렬하거나 손실된 packet을 recover해야 합니다.

---

No call setup at Network Layer:
- Router에는 end-to-end 연결의 어떤 상태도 저장하지 않습니다.
- Router에는 네트워크 개념의 **connection**이 존재하지 않습니다.

Packet은 destination host address를 통해 forwarding됩니다.

---

**IP Nexthop Routing**과 **Forwarding**으로 패킷이 교환됩니다:
- router는 routing을 위한 end-to-end path를 결정하지 않음
- routing decision은 그저 다음 router로 가는 것 (next hop)

Routing protocol을 통해 forwarding table이 생성돼서 routing decision이 결정되고, forwarding은 forwarding table을 통해 단순히 매칭시켜서 다음 router로 packet을 이동시킵니다.

---

### VC와 Datagram 비교

**VC**는 signaling을 통해 경로를 설정하고 중간의 router가 연결 상태를 유지하기 때문에, 네트워크 내부에 control logic이 존재합니다. <br>
즉, edge(end system)는 단순하게 유지됩니다.

**Datagram**은 현재의 인터넷 방식으로, 네트워크 내부가 단순하게 유지됩니다. Router는 각 패킷을 독립적으로 처리하고, IP 주소 기반으로 next hop을 결정하는 단순 forwarding 기능만을 수행합니다. 따라서 IP 네트워크는 router에게 복잡한 일을 시키지 않아서 network core를 단순하게 유지합니다. <br>
이 방식은 반대로 end system을 복잡하게 만듭니다. 패킷 손실, 순서 뒤바뀜 등의 문제의 책임을 end system에게 넘겨서 edge는 이런 문제를 해결하는 복잡한 기능을 구현해야 합니다.

복잡성을 네트워크 코어에 위치하는 것은 유연성과 확장성이 떨어지므로 현대의 인터넷은 **Datagram** 방식을 채택했습니다.