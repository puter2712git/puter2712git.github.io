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
마지막으로 구한 합 $s$를 비선형 함수 $\sigma$에 통과시켜 뉴런의 최종 출력 $h$를 결정합니다. 이때 비선형 함수 $\sigma ()$를 **activation function**이라고 합니다.

<br>

여러 뉴런이 모여서 하나의 **layer**(계층)를 형성하게 되면, 이 layer는 입력으로 벡터를 받아 출력으로 벡터를 내보냅니다.

$$
\displaylines{
s_j = \mathrm{w}_j^T \mathrm{x} + b_j \to \mathrm{s} = \mathrm{W}^T \mathrm{x} + \mathrm{b} \\
\mathrm{h} = \sigma(\mathrm{s})
}
$$

$j$번째 뉴런에 대한 계산을 모아서 하나의 layer에 대한 식을 유도할 수 있습니다. <br>
$\mathrm{W}$는 모든 뉴런의 가중치 벡터($\mathrm{w}_j$)를 행이나 열로 쌓아서 만든 가중치 행렬입니다. <br>
Activation function $\sigma()$는 각 요소에 독립적으로 적용됩니다.

<br>

**Network**(전체 신경망)은 여러 개의 layer를 순차적으로 연결한 구조로, 이를 통해 복잡한 비선형 변환을 수행합니다. 즉, 여러 개의 layer의 동작이 sequential하게 수행되는 구조입니다.

1st layer: <br>
$$
\displaylines{
\mathrm{s}^{(1)} = \mathrm{W}^{(1)^T} \mathrm{x}^{(0)} + \mathrm{b}^{(1)} \\
\mathrm{x}^{(1)} = \sigma(\mathrm{s}^{(1)})
}
$$

2st layer: (1st layer에서의 출력이 2st layer의 입력으로 사용됨) <br>
$$
\displaylines{
\mathrm{s}^{(2)} = \mathrm{W}^{(2)^T} \mathrm{x}^{(1)} + \mathrm{b}^{(2)} \\
\mathrm{x}^{(2)} = \sigma(\mathrm{s}^{(2)})
}
$$

---

### Multilayer Perceptron (MLP)

**MLP**는 현대 인공신경망의 기본이 되는 아키텍처입니다. <br>
MLP는 세 가지 종류의 계층으로 구성됩니다. <br>
- Input layer (입력 계층)
    - 외부 데이터를 네트워크에 주입
    - 데이터의 feature 개수와 동일한 노드 수
- Hidden layer (은닉 계층)
    - 입력 데이터의 복잡하고 추상적인 패턴을 학습하고 추출
    - 단일 퍼셉트론의 한계인 비선형 문제를 해결할 수 있게 만드는 layer
    - 은닉 계층은 하나 이상 존재함
- Output layer (출력 계층)
    - 네트워크의 최종 결과 출력

- 각 layer는 행렬곱을 수행하고 non-linear activation function을 진행합니다. <br>
- **Feed-forward network**: Loop나 cycle 없이 input에서 output 단방향으로만 정보가 이동 <br>
- **Traning NN**: Training data를 통해 뉴런이 가중치를 학습하는 것을 의미합니다.

---

## Activation Function

만약 중간 layer에서 linear activation function만 사용한다면, 아무리 많은 layer를 쌓더라도 단일 layer와 동일하게 되어 비선형 문제를 해결할 수 없게 됩니다.

### Nonlinear Activation Functions

1. Sigmoid function
    - $f(x) = {1 \over 1 + e^{-x}}$
    - 0과 1 사이의 범위
    - 미분 가능 (backpropagation에 사용 가능)
    - 전통적인 인공신경망에서 사용되었으며, 현재는 binary classification에서 최종 출력 layer에서 사용됨

2. Hyperbolic tangent (tanh)
    - -1과 1 사이의 범위
    - 전통적인 신경망이나 RNN(순환 신경망)에서 사용됨

3. Rectified linear unit (**ReLU**)
    - 양수 입력은 그대로 통과시키고, 음수 입력은 0으로 만듦
    - 현대 딥러닝에서 가장 널리 사용되는 activation function
    - 양수 영역에서는 기울기가 일정하므로 기울기 소실 문제를 크게 완화

<br>

Activation function의 주된 역할은 신경망에 비선형성을 부여하는 것입니다. <br>

- 전통적인 activation function
    - Logistic sigmoid, hyperbolic tangent(tanh)
    - 입력값이 극단적일 때 기울기가 0에 가까워지는 **saturating** 특성을 가짐
    - Backpropagation 시에 가중치 업데이트가 거의 일어나지 않게 돼서 학습에 방해됨

- 최근의 activation function
    - ReLU, leaky ReLU, softplus, ELU
    - 기울기 소실 문제를 완화하기 위해 **non-saturating** 특성을 가짐
    - 이를 통해 깊은 신경망을 효율적으로 학습시킬 수 있음

### Activation at Output Layer

Output layer에서 사용하는 activation function은 해결하려는 문제의 종류에 따라 달라집니다. <br>

- Regression
    - 추가적인 변환을 하지 않고, 이전 layer에서 계산된 가중 합을 그대로 출력합니다.
    - Dimension은 예측하려는 값의 개수와 동일합니다.
- Classification
    - Activation function: Softmax function
    - 이전 layer의 가중 합을 입력받아 각 클래스에 속할 확률로 변환합니다. (모든 출력의 합은 1)
    - Dimension은 분류하려는 클래스의 개수와 동일합니다.

---

## Hyperparameters

- 각 layer의 output vector의 dimension (layer의 노드 수)
    - 노드 수가 많을수록 표현력이 커지지만, 계산량이 늘어나고 overfitting 위험도 커집니다.
- Layer의 개수
    - Hidden layer의 수를 결정합니다.
    - 계층의 수가 많을수록 깊은 신경망이 되어 더 추상적이고 계층적인 특징을 학습할 수 있게 됩니다.
- Activation function
    - 어떤 activation function을 사용할지 결정합니다. (ReLU, tanh, sigmoid, ...)

---

## Training Neural Network Parameters

신경망을 훈련시킨다는 것은 손실 함수(loss function) 값을 최소화하는 최적의 가중치와 bias를 찾는 것과 동일합니다.

- Regression: MSE (Mean Squared Error), ...
- Classification: Cross-entropy loss

<br>

**Optimizer**는 손실 함수를 최소화하기 위해 $W$와 $b$를 어떤 방향으로 얼마나 움직여야 할지 결정하는 알고리즘입니다.

- **SGD** (**Stochastic Gradient Descent**) 및 변형
    - 손실 함수의 기울기를 계산하여 기울기의 반대 방향으로 파라미터를 업데이트
    - Adam, RMSprop, Adagrad 등의 variant 존재

SGD가 파라미터를 업데이트하려면 기울기가 필요합니다. **Back-propagation**(역전파)을 통해 이 기울기를 효율적으로 계산할 수 있습니다.

### Loss Function

Loss function은 NN가 예측한 값이 실제 값과 얼마나 차이가 큰지 측정합니다. 즉, NN가 얼마나 나쁜지를 수치화하는 함수입니다.

> #### Classification 문제에 mean squared error loss를 사용할 수 있을까요?
> 사용할 수 있지만, ideal하지 않습니다. <br>
> MSE는 모든 출력에 대해 penalty를 부여합니다. 이는 오답 클래스에도 오차를 계산하여 학습에 반영하게 되므로, 정답 클래스에 집중해야 할 학습 신호를 희석시키고 효율성을 떨어뜨립니다. <br>
>
> 이런 문제를 해결하기 위해, classification에는 **cross-entropy loss**를 주로 사용합니다.

### Gradient Descent

**Gradient descent**(경사 하강법)는 loss function을 최소화하기 위해 가중치 $\mathbf{w}$를 반복적으로 조정하는 최적화 알고리즘입니다.

$$
\mathbf{w}^{(new)} \leftarrow \mathbf{w}^{(old)} - \alpha \nabla _{\mathbf{w}} L(\mathbf{w})
$$

- $\alpha$ (**Learning rate**, 학습률):
    - 파라미터를 업데이트할 때 이동할 보폭(step size)을 결정하는 하이퍼파라미터
- $\nabla _{\mathbf{w}} L(\mathbf{w})$ (**Gradients**, 기울기):
    - 가중치 $\mathbf{w}$ 지점에서 loss function을 가장 빠르게 증가시키는 방향을 나타내는 벡터
    - 기울기 벡터에 마이너스를 붙여서 가장 빠르게 감소하는 방향으로 이동
    

Gradient가 0에 가까워지면 학습이 멈추거나 매우 느려지게 됩니다. Sigmoid, tanh와 같은 activation function은 saturating하므로 최적 지점이 아닌 곳에서도 기울기가 0에 가까워지는 문제가 있습니다. 따라서 3세대 신경망에서는 activation function으로 **ReLU**를 주로 사용합니다.

#### GD for Neural Networks

Deep NN의 loss function의 기울기는 **back-propagation**(역전파) 알고리즘을 통해 효율적으로 계산됩니다. <br>
하지만 loss function은 **non-convex**(비볼록) 형태입니다. 따라서 경사 하강법은 엄청나게 많은 양의 가중치와 편향을 최적화해야 하며, 이때 global minimum(전역 최소점)이 아닌 local minima(지역 최소점)나 saddle point(안장점)에 쉽게 갇힐 수도 있습니다. 이는 학습이 더 이상 진행되지 못하고 최적의 성능에 도달할 수 없음을 의미합니다. <br>

이런 문제를 해결하기 위해 SGD만 사용하지 않고, **momentum**을 추가하거나 학습률을 적절히 조정하는 다양한 알고리즘이 개발되었습니다.

<br>

##### Momentum-based SGD

**Momentum**은 이전 기울기를 누적하여 다음 이동 방향을 결정하는 방식으로, 물리적인 관성 개념을 도입한 것입니다.

$$
\displaylines{
\mathbf{w} \leftarrow \mathbf{w} + \mathbf{v} \\
\mathbf{v} \leftarrow \alpha \mathbf{v} - \eta \frac{\delta \mathbf{L}}{\delta \mathbf{w}}
}
$$

모멘텀을 도입함으로 oscillation이 감소하고, convergence를 가속화하게 됩니다. 또한 local minimum에 갇히더라도 축적된 모멘텀 덕분에 작은 언덕을 넘을 수 있게 되었습니다.

##### Adaptive Learning Rate

**Adaptive learning rate** 알고리즘은 학습률($\eta$)을 수동으로 튜닝하지 않고 훈련 중에 파라미터별로 학습률을 자동으로 조정합니다. 이를 통해 복잡한 지형에서 더 효율적으로 이동할 수 있게 합니다.

**AdaGrad**:
- 과거 기울기의 제곱을 누적하여 학습률을 조정
- 기울기가 큰(자주 업데이트된) 파라미터는 학습률이 점차 감소
- 드물게 업데이트된 파라미터는 학습률이 커짐

**RMSProp**:
- AdaGrad의 학습률이 너무 빠르게 감소하는 문제를 해결하기 위해 도입
- 이동 평균을 이용해 최근 기울기에 대해 더 큰 가중치를 둠
- Adaptive learning rate를 유지하면서 0으로 빠르게 수렴하는 것을 방지

> **Adam**
> Momentum과 adaptive learning rate의 장점을 결합한 알고리즘입니다. <br>
> 기울기의 이동 평균과 기울기 제곱의 이동 평균 두 가지의 이동 평균을 이용해서 가중치를 업데이트합니다.

---

### Initialization

신경망 학습은 초기 가중치에 대해 민감할 수 있습니다. <br>
- 너무 작은 가중치: 기울기 소실 발생 (학습이 일어나지 않음)
- 너무 큰 가중치: 불안정한 학습 (업데이트가 너무 커짐)

이런 문제를 해소하기 위한 인기 있는 초기화 전략이 존재합니다. 좋은 초기화 전략의 목표는 모든 layer에서 활성화 값의 **variance**(분산)를 일정하게 유지하여, 신호가 네트워크를 통해 적절히 흐르도록 하는 것입니다.
- Xavier (Glorot) initialization
- He initialization

> 현대 알고리즘(adaptive optimizers, batch normalization, residual connection, ...)이 초기화의 민감도를 크게 줄여주지만, 여전히 좋은 초기화는 네트워크가 발산하지 않고 빠르고 효율적으로 최적의 해를 찾도록 돕습니다.