---
title: Deep Neural Network
date: 2025-11-24
categories: [Learning]
tags: ["Machine Learning"]
pin: true
math: true
mermaid: true
---

## Neural Model

**Neuron**이란 입력을 받아서 가중치를 계산하고, **activation function**을 통해 결과를 출력하는 단일 계산 단위입니다. <br>

### McCulloch-Pitts (MCP) Neuron

**MCP Neuron**은 생물학적 뇌 세포의 동작을 단순화한 수학적 모델입니다. <br>

![alt text](/assets/image.png) <br>
실제 뉴런의 동작은 매우 복잡하지만, MCP 뉴런은 이를 간단한 binary 논리 게이트로 표현합니다. 입력 신호들의 가중치들의 합이 일정 임계값을 넘으면 1을 출력하고, 그렇지 않으면 0을 출력하는 방식입니다.

$$
y = \begin{cases}
1, & \text{if } \sum_i w_i x_i \ge \theta \\
0, & \text{otherwise}
\end{cases}
$$

### Perceptron

**Perceptron**은 MCP 뉴런을 기반으로 한 인공신경망의 가장 기본이 되는 모델입니다. <br>

입력 신호들의 가중합이 임계값을 넘으면 1을 출력하고, 그렇지 않으면 0을 출력합니다. 모델의 동작 자체는 MCP 뉴런과 동일하다고 볼 수 있지만, 가장 큰 차이는 perceptron은 스스로 최적의 가중치를 학습할 수 있다는 점입니다. 즉, MCP 뉴런은 스스로 학습하지 않고 정해진 가중치를 통해 결과를 예측하지만 perceptron은 스스로 학습하며 최적의 가중치를 결정합니다.

<br>

Perceptron은 **linearly separable**한 문제만 해결 가능하다는 한계가 존재합니다. 즉, 입력 공간에서 데이터가 놓여 있을 때 단 하나의 직선(또는 평면)으로만 두 개의 클래스를 나눌 수 있음을 의미합니다.

<br>

선형 분리되지 않는 문제로는 XOR function 등이 존재하는데, 이는 단일 perceptron으로는 해결할 수 없습니다. 이러한 한계는 perceptron을 다층 구조로 쌓아서 비선형 패턴을 학습하도록 하여 해결되었습니다. 각 perceptron이 복잡한 비선형 패턴을 학습하게 되어, 결국 선형 분리가 불가능한 문제들 또한 해결할 수 있게 되었습니다. 이는 오늘날 **Deep Learning**의 기초가 되는 핵심 아이디어입니다.


### History of Neural Networks

1. First generation (1958~): perceptron
- XOR problem과 같은 선형 분리 불가능한 문제는 해결 불가능하다고 증명이 나와, 신경망 연구에 큰 침체기를 겪게 됩니다.

2. Second generation (1986~): multilayer perceptrons
- **Back-propagating error signal** 알고리즘이 발견되었습니다. (신경망의 가중치를 효율적으로 학습)
- Hidden layer(은닉층)을 통해 비선형적인 문제를 해결할 수 있게 되었습니다. (Shallow network: 1개의 은닉층만을 사용한 얕은 구조)

3. Third generation (2006~): deep learning
- **Deep belief nets** (Hinton, 2006)
- **Convolutional neural network** (CNN), **Recurrent NN** (RNN), **Transformer**, ... 

---

## Neural Network Architecture

### Artificial Neural Network

인공신경망은 기본적으로 입력 $x$를 받아 $y$라는 출력으로 변환하는 모델입니다.

$$
y = f_{W}(x)
$$

- $x$ (입력): 신경망에 들어가는 데이터
- $y$ (출력): 신경망의 최종 결과
- $f_{W}()$ (함수): 신경망 자체를 의미. 여러 층과 노드, 활성화 함수로 구성된 복잡한 과정을 수행
- $W$ (가중치 매개변수 집합): 함수 $f$의 동작을 결정하는 모든 weights와 bias의 집합. 신경망이 학습을 통해 찾아내는 것

$$
\displaylines{
s = b + w_1 x_1 + w_2 x_2 + \cdots + w_M x_M = \mathrm{w}^T \mathrm{x} + \mathrm{b} \\
h = \sigma(s)
}
$$

위 수식은 $x$(input feature)에 가중치 $w$를 곱한 후, 이들을 전부 더한 후 $b$(bias)를 더합니다. 이 수식은 벡터로 변환하여 간결하게 표현할 수도 있습니다. <br>
마지막으로 구한 합 $s$를 비선형 함수 $\sigma$에 통과시켜 뉴런의 최종 출력 $h$를 결정합니다. 이때 비선형 함수 $\sigma$를 **activation function**이라고 합니다.