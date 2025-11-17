---
title: "Order Relations"
date: 2025-11-17
categories: ["Set and Logic"]
math: true
---

# Partial Order

**equivalence relation**에 대해 짚고 넘어가도록 합시다. **equivalence relation**은 다음 세 가지 properties를 만족하는 relation입니다.
- reflexive
- symmetric
- transitive

이는 등호 $$=$$를 일반화한 것으로, 이를 일반화한 것처럼 이번에는 부등호 $$\leq$$를 일반화 해보도록 합시다.

$$\leq$$은 실수 집합 $$\mathbb{R}$$에서 정의됩니다. 이 relation은 reflexive하고 transitive하지만, symmetric하지 않습니다. <br>
($$\because 2 \leq 5 \; \text{and} \; 5 \nleq 2$$)

이를 정리하면, $$x$$와 $$y$$가 $$\mathbb{R}$$에서 정의되고, 만약 $$x \leq y$$이고 $$y \leq x$$이면 $$x = y$$입니다. 이때 $$\leq$$를 $$\it{antisymmetric}$$하다고 말합니다.

$$
\begin{align*}
& \text{A relation } R \text{ on a set } A \text{ is } \mathbf{antisymmetric} \text{ when} \\
& (\forall x \in A)(\forall y \in A)((xRy \wedge yRx) \to x = y); \\
& \text{that is, if } xRy \text{ and } yRx \text{, then } x = y \text{ whenever } x \text{ and } y \text{ are in } A \text{.}
\end{align*}
$$

이제 $\leq$이 reflexive, antisymmetric, transitive하다는 것을 알게 되었습니다. 이 property들을 통해 *order*를 구성할 수 있고, 마침내 **"less than equal to"**의 개념을 다른 집합에도 일반화할 수 있게 됐습니다. 이때의 기호는 $\leq$ 대신 $\preccurlyeq$를 사용합니다.

집합 $A$에서의 relation $\preccurlyeq$이 reflexive, antisymmetric, transitive하다면 $\preccurlyeq$은 $A$의 **partial order**입니다. 즉, $x, y, z$에 대해 다음이 성립합니다.
1. $x \preccurlyeq x$.
2. $\text{If } x \preccurlyeq y \text{ and } y \preccurlyeq x \text{, then } x = y \text{.}$
3. $\text{If } x \preccurlyeq y \text{ and } y \preccurlyeq z \text{, then } x \preccurlyeq z \text{.}$

만약 $\preccurlyeq$이 집합 $A$의 partial order라면, $(A, \preccurlyeq)$를 **partially ordered set** 또는 **poset**이라고 부를 수 있습니다.
예를 들어, $(\mathbb{R}, \leq)$는 poset입니다.

**total order**라는 개념 또한 존재합니다. $\preccurlyeq$이 $A$의 partial order이고, $(\forall x \in A)(\forall y \in A)(x \preccurlyeq y \vee y \preccurlyeq x)$를 만족하면 $\preccurlyeq$은 $A$의 total order입니다. 마찬가지로 $(A, \preccurlyeq)$이 poset이고 $\preccurlyeq$이 total order라면 $(A, \preccurlyeq)$는 **totally ordered set**라고 부를 수 있습니다.

$\leq$과 $\lt$이 구분되어 있듯 order 또한 **strict order**로 구분됩니다. $(A, \preccurlyeq)$이 poset이고, $x \in A$와 $y \in A$에 대해 $x \preccurlyeq y$이고 $x \neq y$이면, $x \prec y$라고 쓰고 **strict order**라고 합니다. strict order는 reflexive하지 않기 때문에 partial order가 아닙니다.

---

## Maximal, Minimal Element

$\preccurlyeq$이 집합 $A$의 partial order라 두고, $b \in A$ 원소가 있다고 합시다. 이때 모든 $x \in A$에 대해 $b \nprec x$이면 $b$를 **maximal element**라고 합니다. 즉, $A$에는 $b$보다 큰 원소가 존재하지 않습니다.

마찬가지로 **minimal element** 또한 존재합니다.

Let $$A = \left\{2, 3, 4, 5, 6, 8, 9\right\}$$. poset $(A, |)$의
- minimal element: $$\left\{2, 3, 5\right\}$$
- maximal element: $$\left\{5, 6, 8, 9\right\}$$

## Upper, Lower Bound

$\preccurlyeq$이 $A$의 partial order라 두고, $S \subseteq A$, $a \in A$, $b \in A$라고 합시다. 이때,
- $b$가 $(\forall x \in S)(x \preccurlyeq b)$를 만족하면 $b$는 $S$의 **upper bound**입니다.
- $a$가 $(\forall x \in S)(a \preccurlyeq x)$를 만족하면 $a$는 $S$의 **lower bound**입니다.

$$S = \left\{2, 3\right\}$$가 $$A$$의 subset이라고 합시다. 그러면 $$6 \in A$$는 $$S$$의 upper bound이지만, lower bound는 존재하지 않습니다.

**least upper bound**, **greatest lower bound**라는 개념 또한 존재합니다.

$$a$$를 $$S$$의 upper bound, $$b$$를 $$S$$의 lower bound라고 합시다. <br>
만약 $$\ell$$이 $$S$$의 upper bound이고 $$\ell \preccurlyeq b$$일 때, $$\ell$$을 $$S$$의 **least upper bound**라고 합니다. <br>
만약 $$g$$가 $$S$$의 lower bound이고 $$a \preccurlyeq g$$일 때, $$g$$를 $$S$$의 **greatest lower bound**라고 합니다.
