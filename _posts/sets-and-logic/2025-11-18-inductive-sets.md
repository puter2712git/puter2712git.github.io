---
title: "Inductive Sets"
date: 2025-11-17
categories: ["Set and Logic"]
math: true
---

# Successor

number theory는 set theory로 표현이 가능합니다. 먼저 이를 보이기 위해 우리는 각각의 natural number를 set으로 표현해야 합니다.

natural number를 set으로 정의하기 전에, **successor**라는 것을 정의해보도록 하겠습니다. <br>
각 $$x$$ set에 대해, **successor** $$x^{+}$$는 $$x^{+} = x \cup \left\{x\right\}$$로 정의됩니다.

successor의 정의에 의해 다음 세 가지 특징이 성립합니다.
1. $$a \in x^{+} \text{ iff } (a \in x \vee a = x)$$.
2. $$x \in x^{+}$$.
3. $$x \subseteq x^{+}$$.

이러한 successor operation을 이용해서 이제 natural number들을 아래와 같이 구성할 수 있습니다.
- $$0 = \varnothing$$.
- $$1 = 0^{+} = 0 \cup \left\{0\right\} = \left\{0\right\}$$.
- $$2 = 1^{+} = 1 \cup \left\{1\right\} = \left\{0, 1\right\}$$.
- $$3 = 2^{+} = 2 \cup \left\{2\right\} = \left\{0, 1, 2\right\}$$.
- $$4 = 3^{+} = 3 \cup \left\{3\right\} = \left\{0, 1, 2, 3\right\}$$.

위와 같은 natural number를 살펴보면 아래와 같은 재미있는 특징이 성립함을 볼 수 있습니다.
1. $$0 \in 1 \in 2 \in 3 \in 4 \in 5 \in \cdots$$.
2. $$0 \subseteq 1 \subseteq 2 \subseteq 3 \subseteq 4 \subseteq 5 \subseteq \cdots$$.

---

# Inductive Sets

infinite set의 존재는 현대 집합론과 수학에 있어서 매우 중요합니다.

#### **Inifinity Axiom**
There is a set $$A$$ that contains the empty set as an element and whenever $$x \in A$$, then $$x^{+} \in A$$.

infinity axiom을 통해, natural number만을 element로 갖고 있는 set이 있음을 증명할 수 있습니다. ($$\omega$$로 표기합니다) <br>
먼저, 각 natural number가 set에 존재한다고 보장하도록 property를 한 가지 정의합시다. 그러면 이 set은 empty set을 포함할 것이고, successor operation에 대해 닫혀 있을 것입니다.

set $$I$$가 iff
1. $$\varnothing \in I$$,
2. $$(\forall a \in I)(a^{+} \in I)$$, that is, $$I$$ is "closed under successor." <br>
이라면, $$I$$를 **inductive**하다고 말합니다.

다시 말해서, inductive set은 모든 natural number를 포함해야만 합니다. 결론은 infinity axiom은 inductive set이 존재함을 증명합니다.

이를 통해 natural number를 다시 정의할 수 있습니다. <br>
즉, **natural number**는 모든 inductive set의 원소로 존재하는 set입니다.

이제 우리는 natural number만으로 구성되는 set이 존재하는지 증명해야 합니다. 이를 위해, 우리는 다음이 집합임을 증명해야 합니다. <br>
$$\left\{x \;\colon x \text{ is in every inductive set}\right\}$$

infinity axiom에 의해, 우리는 inductive set $$A$$가 존재함을 알 수 있습니다. 그러므로 각 $$x$$에 대해 $$x$$는 모든 inductive set에 속하므로 $$x \in A$$가 성립합니다. 따라서 natural number만으로 구성되는 집합이 존재하는 것을 알 수 있습니다.

$$\omega$$는 natural number들로 구성되는 집합입니다. $$\omega$$를 $$\left\{0, 1, 2, 3, \dots\right\}$$로 표현할 수 있지만, $$\omega$$를 $$x$$가 모든 inductive set에 속할 때 $$x \in \omega$$인 집합이라고 정의합시다.

---

이제 다음의 corollary를 증명해 보려고 합니다. <br>

$$\text{Let } I \subseteq \omega \text{ be an inductive set. Then } I = \omega$$

$I$를 $I \subseteq \omega$인 inductive set이라고 합시다. $I$가 inductive set이고 $\omega$는 $\textit{the smallest inductive set}$이기 때문에 $\omega \subseteq I$가 성립합니다. 따라서 $I = \omega$입니다.

이 corollary는 **principle of mathematical induction**(수학적 귀납법)을 재정의한 것과 같습니다. <br>
명제 $P(n)$이 있다고 가정합시다. 이때, $(\forall n \in \omega)P(n)$이 참임을 귀납법을 통해 증명해 봅시다.

$$I = \left\{n \in \omega \colon P(n)\right\}$$로 두고, 이는 $I \subseteq \omega$입니다. 이때 우리는 $P(n)$을 통해 구성되는 집합 $I$가 귀납적 정의의 초기 조건과 귀납 정의를 모두 만족하는지 증명해야 합니다.
1. $0 \in I$,
2. $(\forall n \in \omega)(n \in I \to n^{+} \in I)$

$I$는 inductive하기 때문에 $I = \omega$입니다.

---

# Transitive Sets

추가로 새로운 정의를 살펴보도록 합시다. 집합 $A$가 있을 때, 만약 $(\forall a \in A)(a \subseteq A)$를 만족한다면 $A$를 **transitive set**이라고 부릅니다. 이는 즉 $A$의 모든 원소가 $A$의 부분 집합임을 의미합니다. 또한 이는 $(\bigcup A \subseteq A)$와 동치입니다.

1. $0 \subseteq 1 \subseteq 2 \subseteq 3$이므로 $$3 = \left\{0, 1, 2\right\}$는 transitive합니다.
2. $$A = \left\{0, 1, 3\right\}$$는 transitive하지 않습니다. $$(\because 2 \in 3 \text{, } 2 \notin A \text{ so } 3 \nsubseteq A)$$

아래는 transitive에 관련된 정의들이다.
1. 만약 $$x$$이 transitive set이라면, $$\bigcup(x^{+}) = x$$이다.
2. 모든 natural number는 transitive set이다. $$(\forall n \in \omega \text{, } n \text{ is transitive})$$
3. $$\forall m, n \in \omega, m^{+} = n^{+}$$이면 $$m = n$$이다.
4. 집합 $$\omega$$는 transitive set이다.
