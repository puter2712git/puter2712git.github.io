---
title: Network Layer - CIDR, DHCP
date: 2025-12-02 02:07:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

## CIDR

**CIDR**(Classless InterDomain Routing):
- 현재 Internet의 address assignment strategy
- IP address의 network portion(prefix)가 arbitrary length를 갖도록 설정 (not classful)

> Example: <br>
> 200.23.19.5 /21의 subnet address는 <br>
> Host part가 21 bit이므로 200.23.16.0 /21이 subnet address입니다.

---

**Address aggregation**(or **Supernetting**):
- **route aggregation**, **route summarization**라고도 불림
- 여러 network의 block address들을 하나의 prefix로 외부에 알릴 수 있음
- subnetting의 반대이므로, supernetting라고 부르기도 함,

> Address aggregation을 하기 전에는, ISP를 통해 각 주소를 그대로 router에 전달하였습니다. <br>
> 이로 인해 routing table이 매우 컸습니다. <br>
> Address aggregation 이후 주소를 하나로 통합해서 외부에 전달하기 때문에 routing table이 매우 줄어들었습니다.

---

**Subnetting**과 **Supernetting**은 다음과 같은 차이가 있습니다: <br>
**Subnetting**:
- 하나의 큰 netwrok class를 작은 sub network로 나눔
- address가 효율적으로 할당되도록 도움

**Supernetting**:
- 연속적인 network number의 block을 assign
- class B network의 exhaustion을 줄임

---

## DHCP

**DHCP**(Dynamic Host Configuration Protocol):
- Host가 network에 진입할 때 자동으로 서버로부터 IP address를 할당받을 수 있도록 하는 방식입니다.
    - Lease(임대) 개념으로, 이 기간을 갱신할 수 있음
    - 할당됐던 주소는 다른 host가 재사용 가능
    - 모바일 사용자에게 네트워크 제공 (더 간단하게)

**DHCP operation**:
- **BOOTP client**와 interoperable (호환)
- UDP를 이용한 connectionless service model

다음과 같은 DHCP operation을 주고 받습니다:
- Host는 **DHCP discover** msg를 broadcast (optional)
- DHCP server는 **DHCP offer** msg로 respond (optional)
- Host는 **DHCP request** msg로 IP address를 request
- DHCP server는 **DHCP ack** msg로 address를 send

**DHCP server**는 다음과 같은 정보를 제공합니다:
- IP address
- subnet mask
- first-hop router address (default gateway)
- DNS server의 이름과 IP address

**DHCP relay**:
- DHCP message는 broadcast와 같은 방식으로 전달됩니다.
- 하지만 broadcast는 router에 막히게 됩니다.
- 만약 DHCP server가 다른 subnet에 위치한다면 router는 relay agent로서 둘을 연결하는 매개체가 됩니다.