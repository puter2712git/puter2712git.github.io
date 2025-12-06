---
title: Network Layer - Data and Control Plane
date: 2025-12-01 23:06:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

## Traditional Router Architecture

전통적인 라우터 구조는 control과 data plane이 tight하게 coupled 되어 있었습니다.
- Hardware for data forwarding (data plane)
- Software for routing & management (control plane)

이런 구조는 vendor(공급업체)가 모두 독점적으로 제공하고 있었습니다. 즉, 라우터의 기능 추가, 업데이트, 유지보수 주기가 모두 공급업체의 개발 주기에 종속되어 버렸습니다.

또한 한 장치에 변경 사항이 생기면 다른 모든 장치에도 동일하게 적용되어야 해서 유연성이 매우 떨어졌습니다. (네트워크의 기능 변경, 혁신적인 기능 도입이 어려움. 운영 및 관리 복잡, 비용 증가.)

---

## SDN

**SDN**(Software Defined Networking)은 network control plane을 forwarding (data) plane으로부터 물리적으로 분리하기 위한 개념입니다.

5G에서는 위 개념을 **CUPS**(Control & User Plane Separation)라는 이름으로 채택되었습니다.

---

두 plane을 분리할 때, control plane을 중앙 집중화되어 작동하도록 합니다. 분리되어 있는 control plane의 **Remote Controller**가 data plane의 local **CA**(Control Agent)와 상호작용합니다.

**Data Plane**:
- End device끼리 packet을 네트워크를 통해 교환 (forwarding)
- Router가 핵심 역할을 함
- IP Addresses - IPv4, subnets, subnetting, CIDR, supernetting
- Address related NL protocols - DHCP, NAT
- IPv6

**Control Plane**:
- Routing algorithms - Link-State and Distance Vector
- Routing protocols - RIP, OSPF, BGP
- Supplementary protocols - ICMP and IGMP

**SDN**(Software Defined Networking):
- SDN data plane: Data Forwarding
- SDN control plane: Controller and OpenFlow Protocol

---

## The Internet Protocol (IP)

**Datagram**은 network-layer에서의 packet을 의미합니다. Internet에서의 datagram은 매우 중요한 역할을 하고 있습니다.

**IPv4 Datagram Format**:
- **Version number**
    - datagram의 IP protocol version을 특정
- **Header length**
    - IP datagram header의 길이
    - IP datagram은 options을 가질 수 있지만, 대부분의 datagram은 따로 option을 설정하지 않아서 일반적으로 길이는 5로 설정
- **Type of service**(ToS)
    - datagram의 service 레벨을 특정
- **Datagram length**
    - IP datagram의 전체 길이 (header + data)
- **Time-to-live**(TTL)
    - forward 될 수 있는 최대 횟수 (remaining hops)
    - router에 도달할 때마다 1씩 줄어듦
    - TTL이 0에 도달하면 datagram은 drop됨
- **Protocol**
    - IP datagram이 통과되어야 하는 특정 transport-layer protocol (ex. TCP, UDP)
- **Header checksum**
     - IP datagram header의 bit error를 감지하기 위함

---

IP datagram이 link의 MTU를 초과하면 divided(**fragmented**) 되어 여러 datagram(fragment)로 전송됩니다. <br>
이렇게 나누어진 fragment는 최종 destination에서 **reassembled**됩니다.

IP datagram header의 fragment 관련 flag를 줄 수 있습니다. Fragment 되는 것을 막아두었는데 fragment가 필요한 상황이라면 datagram이 discard 됩니다.

> Example: <br>
> MTU = 1500 bytes, router가 4000 bytes의 datagram을 전달받았을 때, <br>
> fragment는 세 개 생성됩니다.
>
> 원본 datagram: 20 bytes(header) + 3980 bytes(data) <br>
> 헤더 크기가 20 bytes이므로 각 fragment는 최대 1480 bytes의 data를 가질 수 있음 <br>
> fragmentation offset은 8의 배수여야 하므로 data의 크기는 항상 8의 배수여야 합니다. (1480 % 8 = 0) <br>
> 각 fragment의 ID는 원본 datagram과 같은 ID를 가짐 <br>
> fragment의 frag 비트는 1로, more 비트는 마지막 비트만 0으로 설정 <br>
> offset은 이전 segment의 data 크기를 8로 나눈 값입니다.

---

## IPv4 Addressing

**IP Address**: Internet에 연결되어 있는 컴퓨터의 각 **network interface**가 가지는 unique identifier

여기서 **interface**란 host, router와 physical link 사이의 connection을 의미합니다.
- Router는 여러 개의 interface를 가질 수 있습니다.
- Host는 보통 1개 또는 2개의 interface를 가집니다.

**IPv4 Address**: 32 bit의 dotted decimal notation <br>
11000001.00100000.11011000.00001001 $\to$ 193.32.216.9

**IPv6 Address**: 128 bit, hexadecimal, colon으로 분리됨 <br>
3ffe:1900:65455:3:230:f804:7ebf:12c2

---

IP address는 다른 interface와 연관되어 있습니다. <br>
Host가 여러 개의 NIC를 가지고 있으면, 각 NIC(Network Interface Card)는,
- 다른 network address를 가지는 unique IP address를 가집니다.
- 해당 NIC에만 연결된 특정 network에만 연결됩니다.

---

IPv4 Address는 두 부분으로 나뉩니다:
- Network Part(Network Address)
    - NIC가 어디에 연결되어 있는지 구별합니다. (Internet에서 중복되지 않음)
- Host Part
    - 네트워크의 어느 particular system인지 구별합니다. (Network에서 중복되지 않음)

하나의 router interface에 연결되어 있는 모든 host는 같은 network address(prefix)를 가집니다.

---

왜 IPv4 Address를 두 부분으로 나누었을까요?:
- 이는 routing table을 관리할 때 효율성을 높이기 위함입니다.
- Routing table을 network address와 interface 두 가지 column으로 나눠서 관리하게 되면 packet이 어느 router로 가야 하는지 빠르게 판단이 가능합니다.

---

**Classful IPv4 Addressing**:
- 초기의 IP addressing architecture
- IP address는 두 부분을 가집니다: network와 host
    - 이때는 network number는 상위 octet에 해당
- A, B, C, D, E의 다섯 개의 class
    - A, B, C는 universal unicast addressing을 위함
    - D는 multicast를 위함
    - E는 나중을 위한 구현체

---

**Unicast**: A, B, C class의 addressing 방식
- point-to-point(1:1) connection
- 두 unicast host 간 양방향 연결

**Broadcast**: Host 부분이 모두 1을 가지는 경우 (255.255.255.255)
- point-to-multipoint(1:N) unidirectional connection
    - broadcast message는 LAN segment의 모든 host에게 동시에 전송됨
    - LAN router에 막힘

**Multicast**: D class의 addressing 방식
- 224.0.0.0 ~ 239.255.255.255
- one-to-many(1:N) unidirectional connection for group communications
    - multicast group의 어떤 member의 multicast message는 같은 multicast address로 모든 member에게 전송됨
    - multicast message는 router로 전달될 수 있음
- multicast group 안의 host는 같은 multicast address를 가짐

---

**Public IP Address**
- InterNIC(현재는 IANA)에 의해 unique하게 할당됨
- 모든 Internet host와의 end-to-end global 연결을 위함
- Public IPv4 address exhaustion problem 존재
- solution technologies: CIDR, NAT, IPv6, private IP addresses

Why **Private IP Address**??:
- Internet Protocol과 application의 개발을 위해 설계됨
- Private LAN의 host는 public IP address가 필요하지 않음 (public에 접속할 필요가 없을 경우)

**Private IP Address**:
- IANA에 의해 할당되지 않음
- Internet backbone에 route되지 않은, network를 위해서 사용
- Internet과 직접적으로 연결 불가능
- 외부 host에 접근 불가능

아래는 클래스마다 할당되는 private address 범위입니다:
- A: 10.0.0.0 ~ 10.255.255.255
- B: 172.16.0.0 ~ 172.31.255.255
- C: 192.168.0.0 ~ 192.168.255.255

Private IP adderss가 외부 public network에 접근하려면 **NAT**(Network Address Translation)이 필요합니다.

---

IANA는 특별한 목적의 IPv4 address를 등록했습니다:
- '0' = **this** (0.0.0.0 - this host on this network)
- '1' = **all** (255.255.255.255 - all hosts on all networks broadcast)
- 127.0.0.0/8 - for loopback