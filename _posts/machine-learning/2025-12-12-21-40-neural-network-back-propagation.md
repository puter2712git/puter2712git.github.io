---
title: Neural Network - Back-Propagation
date: 2025-12-12 21:40:00 +0900
categories: [Machine Learning]
tags: ["Learning", "Machine Learning"]
pin: true
math: true
mermaid: true
---

## Back-Propagation

**Neural Network**(NN)는 **loss function**을 최소화하는 방향으로 스스로 학습합니다. 이런 loss function을 최소화하기 위해 **Gradient Descent** 방법을 사용합니다. 이 경사 하강법을 사용하려면 경사, 즉 가중치와 편향에 대한 loss function의 미분 값을 알아야 합니다.

$$
L(W) = \sum_{i}{((W_{2} \sigma (W_{1}x_{i} + b_{1}) + b_{2}) - y_{i})^{2}}
$$

위와 같은 loss function에서, 다음의 미분 값들을 구해야 합니다:

$$
\frac{\partial L}{\partial W_{1}},\; \frac{\partial L}{\partial W_{2}},\;
$$