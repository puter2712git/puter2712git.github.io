---
title: Context-Free Grammars Simplification
date: 2025-12-06 21:29:00 +0900
categories: [Theory of Computation]
tags: ["Learning", "Theory of Computation"]
pin: true
math: true
mermaid: true
---

## Substitution Rule

**Substituion Rule**은 다음과 같습니다:

$G = (V, T, S, P)$ 를 CFG라고 둡니다. 이때, $P$ 를 다음과 같은 production이라고 가정합니다.

$$
A \to x_{1}Bx_{2}
$$

$$
B \to y_{1} \;\vert\; y_{2} \;\vert\; \cdots \;\vert\; y_{n}
$$

$G' = (V, T, S, P')$ 를 CFG로 두고, $P'$는 기존 $A \to x_{1}Bx_{2}$ 를 $A \to x_{1}y_{1}x_{2} \;\vert\; x_{1}y_{2}x_{2} \;\vert\; \cdots \;\vert\; x_{1}y_{n}x_{2}$ 로 바꾼 production이라고 둡니다. 이때, $L(G') = L(G)$ 이 성립합니다.

---

위는 다음의 내용을 보이는 것으로 증명할 수 있습니다:
- If $w \in L(G)$, then $w \in L(G')$
- If $w \in L(G')$, then $w \in L(G)$

$w \in L(G)$ 라고 가정하면, $S \overset{\ast}{\underset{G}\implies} w$ 이 성립합니다.

만약 이 derivation이 production $A \to x_{1}Bx_{2}$ 를 사용하지 않는다면, 이는 $S \overset{\ast}{\underset{G'}\implies} w$ 로 바로 표현할 수 있습니다.

만약 해당 production을 사용한다면, $A \to x_{1}Bx_{2}$ production을 사용했다고 해봅시다. 그러면 다음과 같은 rewriting이 발생한다고 가정할 수 있습니다.

$$
S \overset{\ast}{\underset{G}\implies} u_{1}Au_{2} \underset{G}\implies u_{1}x_{1}Bx_{2}u_{2} \underset{G}\implies u_{1}x_{1}y_{j}x_{2}u_{2}
$$

또한 $G'$ 를 통해 다음과 같은 rewriting을 유도할 수 있습니다.

$$
S \overset{\ast}{\underset{G'}\implies} u_{1}Au_{2} \underset{G'}\implies u_{1}x_{1}y_{j}x_{2}u_{2}
$$

이를 통해 $G$ 와 $G'$ 가 같은 sentential form으로 도달한다는 것을 확인할 수 있습니다. 만약 다시 $A \to x_{1}Bx_{2}$ production을 사용하면, 이 방식을 반복하면 됩니다.

그러므로 이 rewriting steps를 거쳐가면 다음을 얻습니다.

$$
S \overset{\ast}{\underset{G'}\implies} w
$$

따라서, 만약 $w \in L(G)$ 이면, $w \in L(G')$ 가 성립합니다.

---

다음은 substitution rule을 적용하는 예시입니다.

$G = (\lbrace A, B \rbrace, \lbrace a, b, c \rbrace, A, P)$ 이고, $P$를
- $A \to a \;\vert\; aaA \;\vert\; abBc$
- $B \to abbA \;\vert\; b$

로 둡니다.

이때 substitution rule을 적용하면 새로운 $P'$ 를 다음과 같이 유도할 수 있습니다.
- $A \to a \;\vert\; aaA \;\vert\; ababbAc \;\vert\; abbc$
- $B \to abbA \;\vert\; b$ (*unnecessary*)

---

CFG인 $G = (V, T, S, P)$ 의 symbol이 **redundant**한 경우는 다음과 같습니다:
- (**nonterminating**) if it is unable to derive any terminal strings
- (**unreachable**) if it does not appear in any sentential form

$T$ 의 모든 terminal은 항상 **terminating**해야 하고, $S$ 는 항상 **reachable** 해야 합니다.

---

## Terminating Symbols

만약 $V$ 에 속하는 variable(nonterminal) $A$ 가 terminating symbols만으로 구성되는 production이 존재하면, $A$ 는 **terminating**한다고 정의합니다.

CFG $G = (V, T, S, P)$ 가 주어졌을 때,
- $P$ 의 모든 terminal를 mark합니다.
- $Q$ 는 적어도 하나의 right-hand side가 marked symbols로 구성되어 있는 unmarked nonterminal들의 집합입니다.
- $Q$ 의 symbol 중 $P$ 에서 mark한 것들을 mark합니다. (반복)

---

$$
S \to ASB \;\vert\; BSA \;\vert\; SS \;\vert\; aS \;\vert\; \lambda
$$

$$
A \to AB \;\vert\; B
$$

$$
B \to BA \;\vert\; A
$$

위 production들 중, 먼저 terminal symbol을 찾아 표시합니다:
- $a, \lambda$ mark

이제 nonterminal들 중, right-hand side가 marked symbol로만 구성되어 있는 symbol을 mark합니다:
- $S$ 의 RHS 중 $\lambda$ 로만 이루어진 RHS가 존재하므로, $S$ mark

---

## Reachable Symbols

$P$ 에 $A \to \alpha$ 와 같은 production이 주어졌다고 합시다. 만약 $A$ 가 **reachable**하면, $\alpha$ 의 모든 symbol은 reachable합니다.

다음과 같은 $P$ 가 있다고 합시다.

$$
S \to aS \;\vert\; SB \;\vert\; SS \;\vert\; \lambda
$$

$$
A \to ASA \;\vert\; C
$$

$$
B \to b
$$

1. 순차적으로 $P$ 의 모든 reachable symbol을 mark합니다.
    - $S$
    - $S,\; a,\; B$
    - $S ,\; a,\; B,\; b$
2. Unreachable(unmarked) symbol들을 제거합니다.

---

## Simplification-Redundant Symbols

**Redundant Symbol**이란, **nonterminating** 또는 **unreachable**한 symbol을 의미합니다.

CFG $G$ 가 redundant symbol을 갖고 있지 않으면 $G$ 가 **reduced** 되었다고 정의할 수 있습니다.

---

**Empty production**은 righthand side가 empty string인 production입니다. 즉, 다음과 같은 형태의 production을 의미합니다.

$$
A \to \lambda
$$

CFG $G = (V, T, S, P)$ 가 주어지고, $\lambda \notin L(G)$ 이면, $G$ 의 어떤 empty production이든 $L(G)$ 에 영향을 끼치지 않기 때문에 제거할 수 있습니다.

만약 variable(nonterminal) $A$ 에 다음과 같은 derivation이 존재하면, $A$ 를 **$\lambda$-variable**라고 부릅니다.

$$
A \overset{+}\implies \lambda
$$

$G$ 가 $\lambda$-production을 가지고 있지 않으면, $G$ 를 **$\lambda$-free**하다고 합니다.

---

만약 $G$ 가 $\lambda$-production을 가지고 있을 때, 다음과 같은 derivation을 살펴봅시다.

$$
S \implies \alpha_{1} \implies \alpha_{2} \implies \cdots \implies \alpha_{n}
$$

$\alpha_{i}$ 의 length는 다른 것과 비교할 때 매우 다양해질 것입니다. ($\lambda$ 가 포함되기 때문)

하지만 $G$ 가 $\lambda$-free하다면, 다음과 같은 식이 성립합니다.

$$
\vert S \vert = 1 \le \vert\alpha_{1}\vert \le \vert\alpha_{2}\vert \le \cdots \le \vert\alpha_{n}\vert
$$

즉, length는 **monotonically increasing**합니다. 이는 우리가 해당 string이 CFG로 생성되었는지 아닌지를 확인할 수 있도록 도와줍니다.

---

### Removing $\lambda$-Productions

$\lambda$-production이 존재하는 CFG $G = (V, T, S, P)$ 가 주어졌다고 할 때, 이 CFG와 equivalent한 $\lambda$-free인 CFG $G'$가 존재합니다.

1. 먼저 $G$ 의 모든 $\lambda$-variables의 집합 $V_{N}$ 를 찾습니다.
    1. $A \to \lambda$ 인 production을 찾아서 $V_{N}$ 에 추가합니다.
    2. $V_{N}$ 더 이상 variable이 추가되지 않을 때까지 반복합니다: $B \to A_{1}A_{2} \cdots A_{n}$, where $A_{1},\; A_{2},\; \cdots,\; A_{n} \in V_{N}$ 와 같은 새로운 variable을 $V_{N}$ 에 추가합니다.
2. 각 $\lambda$-variable에 대해 이를 대체하는 새로운 production을 생성해서 추가합니다.
3. $P$ 의 모든 $\lambda$-production을 제거합니다.
4. 모든 nonterminating symbol을 제거합니다.

---

$P$가 다음과 같이 주어집니다.

$$
S \to aS \;\vert\; SS \;\vert\; bA
$$

$$
A \to BB
$$

$$
B \to CC \;\vert\; ab \;\vert\; aAbC
$$

$$
C \to \lambda
$$

이때 $V_{N}$ 은 다음과 같이 구성됩니다: $V_{N} = \lbrace C,\; B,\; A \rbrace$

그럼 각 $\lambda$-production의 제거를 통해 새로운 production을 추가할 수 있습니다.

$$
S \to aS \;\vert\; SS \;\vert\; bA \;\vert\; b
$$

$$
A \to BB \;\vert\; B
$$

$$
B \to ab \;\vert\; aAbC \;\vert\; abC \;\vert\; aAb
$$

$$
C \to \lambda
$$

이제 $\lambda$-production과 nonterminating symbol을 삭제합니다.

$$
S \to aS \;\vert\; SS \;\vert\; bA \;\vert\; b
$$

$$
A \to BB \;\vert\; B
$$

$$
B \to ab \;\vert\; aAb
$$

---

$P$ 가 다음과 같이 주어집니다.

$$
S \to ABaC
$$

$$
A \to BC
$$

$$
B \to b \;\vert\; \lambda
$$

$$
C \to D \;\vert\; \lambda
$$

$$
D \to d
$$

$V_{N}$ 은 다음과 같습니다: $V_{N} = \lbrace B,\; C,\; A \rbrace$

새로운 production을 추가하고 $\lambda$-production을 삭제합니다:

$$
S \to ABaC \;\vert\; BaC \;\vert\; AaC \;\vert\; ABa \;\vert\; aC \;\vert\; Ba \;\vert\; Aa \;\vert\; a
$$

$$
A \to BC \;\vert\; C \;\vert\; B
$$

$$
B \to b
$$

$$
C \to D
$$

$$
D \to d
$$

---

### Removing Unit Productions

CFG $G = (V, T, P, S)$ 가 주어질 때, $A,\; B \in V$ 인 production $A \to B$ 를 **unit production**이라고 합니다. $G$ 의 unit production을 삭제하기 위해 substitution rule을 사용할 수 있습니다.

$\lambda$-production이 없는 CFG $G = (V, T, P, S)$ 가 주어질 때, unit production이 없는 equivalent한 CFG $G' = (V', T, S, P')$ 가 존재합니다.

---

CFG $G = (V, T, S, P)$ 가 주어지고, 다음과 같은 $P$ 가 주어졌다고 합시다:
- $S \to Aa \;\vert\; B$
- $A \to a \;\vert\; bc \;\vert\; B$
- $B \to A \;\vert\; bb$

그럼 다음과 같은 unit production을 찾을 수 있습니다:
- $S \overset{\ast}\implies A$
- $S \overset{\ast}\implies B$
- $A \overset{\ast}\implies B$
- $B \overset{\ast}\implies A$

이 unit production에 대한 새로운 production을 구성하면 다음과 같습니다:
- $S \to a \;\vert\; bc \;\vert\; bb$
- $A \to bb$
- $B \to a \;\vert\; bc$

그러므로, unit production을 제거한 기존 CFG와 equivalent한 grammar의 production은 다음과 같습니다:
- $S \to a \;\vert\; bc \;\vert\; bb \;\vert\; Aa$
- $A \to bb \;\vert\; a \;\vert\; bc$
- $B \to a \;\vert\; bc \;\vert\; bb$

---

### CFG Simplifications

CFG가 주어졌을 때, redundant symbol, $\lambda$-production, unit production이 존재하지 않는 equivalent한 CFG가 존재합니다.

이는 다음을 수행해서 simplified CFG를 얻는 것으로 증명할 수 있습니다:
- $\lambda$-production 제거
- unit production 제거
- redundant symbol 제거

