---
title: Network Layer - Subnet and Subnetting
date: 2025-12-02 01:31:00 +0900
categories: [Computer Network]
tags: ["Computer Network"]
pin: true
math: true
mermaid: true
---

## Subnets

**Subnet**이란 다음과 같은 device interface들의 group을 의미합니다:
- 서로 physically 도달할 수 있음
- 같은 network address를 가짐
- 따로 중간에 개입하는 router가 없음

하나의 **router interface**가 **subnet**를 이룹니다. <br>
Message는 subnet을 통해 모든 interface에게 broadcast 됩니다.

> Example: <br>
> 만약 세 개의 router로 연결되어 있는 네트워크가 있을 때, subnet의 개수는 <br>
> 각 router로 묶여 있는 네트워크의 합과, router끼리를 연결하는 link의 합으로 총 6개입니다.

---

**Subnet Mask**는 subnet의 network portion을 표현합니다. Subnet의 모든 interface는 같은 network part(network address)를 가집니다. 즉, subnet의 각 IP address는 같은 subnet mask를 가집니다.

Subnet mask는 다음과 같이 표현됩니다:
- Network portion에는 1로, host part에는 0으로 표시
- **/number**는 network portion의 비트의 개수

> Example: <br>
> Classful IPv4 address의 subnet mask는 <br>
> class A: 255.0.0.0 (11111111.00000000.00000000.00000000) $\to$ /8 <br>
> class B: 255.255.0.0 (11111111.11111111.00000000.00000000) $\to$ /16 <br>
> class C: 255.255.255.0 (11111111.11111111.11111111.00000000) $\to$ /24

---

각 subnet은 **Subnet Address**를 가집니다. <br>
이때 **subnet mask**를 이용해 **network address**를 표현합니다.

(ex. 223.1.1.4 router의 subnet address: 223.1.1.0 /24)

Subnet address의 계산 방법은 다음과 같습니다:
NIC IP address가 210.104.35.3 /24인 subnet address를 구하려면, <br>
- 먼저 **210.104.35.3**과 subnet mask인 **255.255.255.0**을 **AND**합니다.
    - Result: 210.104.35.0
- 위의 결과인 210.104.35.0에 subnet mask까지 표현하여 subnet address를 구합니다.
    - Result: 210.104.35.0 /24

---

## Subnetting

**Why subnetting?**:
- B class의 네트워크는 65,535($2^{16}$)개의 address를 가질 수 있습니다.
- 이는 collision possibility와 management complexity를 증가시킵니다.
- IPv4 address exhaustion

**Subnetting**이란 하나의 IP network address를 작은 여러 개의 subnetwork로 나누는 기법입니다.
- 모든 host가 subnet addressing을 지원해야 하도록 규정되었습니다. (RFC 950)
- Subnetting을 통해 site가 하나의 internet address를 통해 여러 physical network에 공유될 수 있게 됐습니다.

Subetting에 의해 subnet mask의 구성도 달라졌습니다.

> 기존 class B는 255.255.0.0 /16 형태의 subnet address를 가졌습니다. <br>
> 여기서 subnetting을 적용하여 host 부분에서 세 개의 bit를 subnetting에 사용합니다. <br>
> 그럼 subnet address는 255.255.224.0 /19가 됩니다. (11111111.11111111.11100000.00000000)

Subnet bit와 host bit 수를 통해 사용 가능한 subnet과 host의 수를 알 수 있습니다:
- `b`: subnetting을 위한 borrowed bits
    - 최대 사용 가능한 subnets = $2^{b}$
- `r`: remaining host bits
    - subnet마다 최대 host 수 = $2^{r} - 2$
    - 2를 빼는 이유는 모두 0인 경우(this)와 모두 1인 경우(all)를 제외

> Example: <br>
> 다섯 개의 subnet을 생성하려면, 최소 몇 개의 bit를 빌려야 할까? <br>
> 1 bit: $2^{1} = 2$ <br>
> 2 bits: $2^{2} = 4$ <br>
> 3 bits: $2^{3} = 8$ <br>
> 따라서 최소 3개의 비트를 host bit로부터 빌려야 합니다.

> Example: <br>
> class B network에 255.255.248.0 subnet mask를 적용하면 <br>
> 몇 개의 usable subnet이 만들어질까? <br>
> subnet mask를 적용하면 5개의 bit를 빌리게 됨 <br>
> 따라서 $b = 5$, $r = 11$ <br>
> usable subnet 수 = $2^{b} = 2^{5} = 32$ <br>
> usable hosts in each subnet = $2^{r} - 2 = 2^{11} - 2 = 2048 - 2 = 2046$