---
title: Context-Free Languages
date: 2025-12-05 00:24:00 +0900
categories: [Theory of Computation]
tags: ["Learning", "Theory of Computation"]
pin: true
math: true
mermaid: true
---

## Context-Free Languages

**Context-Free Language**는 Regular Language 바로 위의 단계에 위치하는 언어입니다.

**PDA**(Pushdown Automata)는 NFA에 **stack**이 붙어있는 형태의 추상 기계 모델로, CFL을 인식할 수 있습니다. <br>
CFL은 무한한 기억 능력이 필요한 패턴을 처리해야 하기 때문에, PDA의 스택을 통해 이러한 구조를 기억하고 처리할 수 있습니다.

---

**Grammar**는 language를 정의합니다: (C, Java, Python, English, Korean, ...)

Grammar의 구조는 다음과 같이 정의될 수 있습니다:
- <sentence\> $\to$ <noun-phrase\> <predicate\>
- <noun-phrase\> $\to$ <article\> <noun\>
- <predicate\> $\to$ <verb\>

- <article\> $\to$ a, the
- <noun\> $\to$ cat, dog
- <verb\> $\to$ runs, sleeps

위 Grammar로 만들 수 있는 language는 다음과 같습니다.

$$
\begin{aligned}
L = \{ \text{a cat runs}, \text{a cat sleeps}, \text{the cat runs}, \text{the cat sleeps}, \\
\text{a dog runs}, \text{a dog sleeps}, \text{the dog runs}, \text{the dog sleeps} \}
\end{aligned}
$$

위 grammar에서 <sentence\>, <noun-phrase\>, <predicate\>는 **variables**(**nonterminals**), a, the, cat, dog, runs, sleeps와 같은 단어는 **terminals**라고 부릅니다.

---

Grammar는 **rewriting**을 기반으로 하는 **language-generation method**입니다.

> Let $\sum = \{a, b, c\}, c \to acb$ be a rewriting rule and $c$ be the starting string. <br>
> Then, we can rewrite the start string as follows:
>
$$
c \Rightarrow acb \Rightarrow a^{2}cb^{2} \Rightarrow a^{3}cb^{3} \Rightarrow \cdots
$$
>
> 위 규칙을 통해 $i \ge 0$, $a^{i}cb^{i}$ 형태의 모든 string을 구성하는 language를 얻을 수 있고, 이는 nonregular language입니다. <br>
> 이러한 경우, rewriting은 **context-free rewriting**입니다.

> Let $\sum = \{a, b, c\}$, $ab \to aabbc$ and $cb \to bc$ be two rewriting rules and $abc$ the starting string. Then, we can rewrite the starting string as follows:
>
$$
abc \Rightarrow aabbcc \Rightarrow aaabbcbbc
$$
>
> 위 두 가지 rewriting rules에 의해 rewrite의 choice가 생깁니다:
>
$$
a^{2}abbcbcc \Rightarrow a^{3}abbcbcbcc \Rightarrow \cdots
$$
>
> 또는,
>
$$
a^{2}abbcbcc \Rightarrow aaabbbccc \Rightarrow \cdots
$$
>
> 위 grammar에서, $a^{*}b^{*}c^{*}$ 형태의 string만 고려한다면 $\{a^{i}b^{i}c^{i} | i \ge 1\}$ language를 얻을 수 있습니다. <br>
> 위 grammar의 langauge는 nonregular하고 noncontext-free인 language 중 하나로 알려져 있습니다.
