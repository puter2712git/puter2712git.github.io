---
title: Network Layer - NAT, IPv6
date: 2025-12-02 03:03:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

이전에 public IP address는 global end-to-end 연결이 가능하지만, private IP address로는 private network의 연결만 가능하다는 것을 알게 됐습니다.

## NAT

**NAT**(Network Address Translation)은 하나의 public IP address를 local domain의 기기에 부여된 다른 private IP address들과 매핑하는 방법론입니다.

외부 네트워크 세상을 고려해서, local network는 하나의 IP address만을 사용하는 것이 옳습니다.

Features:
- ISP로부터 특정 범위의 address가 필요하지 않음; 모든 기기에 하나의 address만 부여해도 충분
- 밖에 따로 알리지 않아도 local network 안의 기기의 address를 변경해도 상관없음
- Local network의 기기의 address를 변경하지 않아도 ISP를 변경 가능
- Local network의 기기는 외부에서 addressable 하지 않고, visible 하지 않음 (security)

**NATs**은 집이나 개인 network, 4G/5G cellular network와 같은 곳에서 방대하게 사용됩니다.

---

## IPv6

**IPv6**는 32-bit의 **IPv4** address space의 고갈 문제를 해소하기 위해 등장했습니다. 이외에도 형식을 바꿀 때 추가적으로 고려된 부분이 존재합니다:
- Header format을 변경해서 processing/forwarding 속도를 높이기
- Header를 변경해서 QoS(Quality of Service)를 촉진시키기

**Comparisions with IPv4**:

- Address
    - IPv4: 32 bits (4 bytes)
    - IPv6: 128 bits (16 bytes)
- Address Space
    - IPv4: $2^{32}$ (Over $10^{9}$)
    - IPv6: $2^{128}$ (Over $10^{38}$)
- Packet Header
    - IPv4: Variable size (상대적으로 시간 소요 있음)
    - IPv6: Fixed size (40 bytes) (더 효율적)
- Address Allocation
    - IPv4: Traditionally A, B, C class. Currently CIDR (but flat)
    - IPv6: CIDR based, hierarchy by geographic region
- Address Type
    - IPv4: Unicast, Multicast, **Broadcast**
    - IPv6: Unicast, Multicast, **Anycast**
- QoS
    - IPv4: Defined (TOS), but not generally used
    - IPv6: Traffic Class, Flow Label
- Security
    - IPv4: Limited and Separated
    - IPv6: IPSec built-in
- Configuration
    - IPv4: Manual Configuration
    - IPv6: Auto Configuration

---

**IPv6 Address**는 hexadecimal로 쓰여지고, colon으로 구분됩니다. <br>
연속적인 zero는 "::"로 대체됩니다: <br>
3ffe:3700:0200:00ff:0000:0000:0000:0001 <br>
$\to$ 3ffe:3700:200:ff:0:0:0:1 <br>
$\to$ 3ffe:3700:200:ff::1

---

IPv6 Address의 세 가지 categories:
- **Unicast** (one-to-one, bi-directional)
    - Address for a single interface
    - delivery to designated one address
- **Multicast** (one-to-many, unidirectional)
    - 같은 membership의 interface를 위한 address
    - 해당 address를 통해 모든 member에게 message 전달
    - No Broadcast
    - IPv6에서는 broadcast 대신 multicast를 사용
- **Anycast** (one-to-nearest one, bi-directional)
    - interface 그룹을 위한 address
    - 해당 address로 식별되는 아무 host에게 message 전달

**Anycast Address**는 hosts의 group을 나타내는 address입니다. <br>
Datagram은 group의 아무나에게 전달됩니다. (nearest host)

---

IPv6 Address의 세 가지 주요 scope:
- **Link-Local**: 단일 **DataLink Layer** 내의 모든 인터페이스
- **Site-Local**: 하나의 관리 사이트(또는 도메인)에 도달 가능한 모든 인터페이스
- **Global**: Internet을 통해 접근 가능한 모든 인터페이스

---

**IPv6 Global Unicast Address(GUA) Format**:
- Internet을 통해 접근 가능한 address (IANA에 의해 할당됨)
- Network Prefix와 Interface ID로 구성됨
    - Classless address로 설계되었고, network portion 대신 network prefix를 사용함
    - Node가 여러 interface를 가질 수 있으며, host portion 대신 interface ID를 사용함
- Network prefix는 길이가 고정되어 있지 않으며, non-self-encoding임 (CIDR와 비슷)
    - **Global Routing Prefix**: ISP 또는 administrative organization에 의해 할당 (보통 48 bits)
    - **Subnet ID**: organization 안의 subnet에 의해 할당 (보통 16 bits)
    - **Interface ID**: 개별적인 host interface에 의해 할당 (MAC address-based)
- **Global Unicast Address**: 세 개의 비트가 **001**로 시작 (2000::/3)

**Interface ID**(64 bits)는 여러 방식으로 할당됩니다:
- 64-bit EUI-64에 의해 자동으로 설정되거나, MAC address로부터 확장
- pseudo-random number에 의해 자동 생성
- DHCP 등에 의해 설정
- 수동으로 설정

---

**IPv6 Datagram**:
- **Base header**: 고정된 40 bytes 크기
- **Extension header**:
    - optional한 Internet Layer 정보를 다루기 위함
    - base header와 upper-layer protocol header 사이에 놓임
    - IPv4 40 byte limit을 없앰

**Base header**를 고정된 40 byte로 설정하여 router에서 빠르게 process되도록 하였고, address size를 크게 늘려서 addressing 공간을 확장했습니다. 또한 **flow labeling**과 **priority** concept가 채택되었습니다.

---

IPv4 address format과 비교하면 다음과 같습니다:
- 중간 router의 fragmentation이 금지됨 (source와 destination 사이에서만 사용)
- processing time을 줄이기 위해 checksum을 삭제
- option field를 없애고 고정된 길이의 40 bytes header를 사용
    - next header가 option을 정의

---

IPv4에서 IPv6로의 전환 전략:
- **Dual Stack**:
    - IPv6 node 또한 IPv4 implementation을 완벽히 지니고 있습니다.
    - 따라서 IPv4와 IPv6 노드 간 datagram 전송이 가능합니다.
- **Tunneling**:
    - IPv6 datagram이 IPv4 router에서는 payload로 인식하여 전송됩니다.
- Header Translation