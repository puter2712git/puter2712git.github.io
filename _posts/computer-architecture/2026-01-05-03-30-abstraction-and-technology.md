---
title: 1. Abstraction and Technology
date: 2026-01-05 03:30:00 +0900
categories: [Computer Architecture]
tags: ["Learning", "Computer Architecture"]
pin: true
math: true
mermaid: true
---

## Eight Great Ideas in Computer Architecture

다음의 여덟 가지 아이디어는 처음 컴퓨터가 발명되었을 때부터 적용되었고, 현대까지 이 아이디어들을 기반으로 컴퓨터가 발전해 왔습니다.

### Moore's Law

**Moore's Law**는 컴퓨터의 급격한 변화를 나타냅니다. 무어의 법칙은 1965년에 Gordon Moore가 예측한 것으로, 18 ~ 24 개월마다 칩에 집적되는 트랜지스터 수가 두 배가 된다는 것입니다. <br>
컴퓨터의 설계는 년에 걸쳐서 진행되기 때문에, 칩의 resource(트랜지스터)는 결과적으로 두 배 혹은 네 배까지 증가해 왔습니다.

### Use Abstraction to Simplify Design

컴퓨터의 설계와 프로그래머는 더 좋은 기술을 개발해야 하는 한편, 무어의 법칙에 의해 트랜지스터의 수가 늘어남에 따라 설계에 걸리는 시간도 급격하게 늘어났습니다. <br>
**Abstraction**은 하드웨어와 소프트웨어 기술 모두에 공통적으로 적용됩니다. Lower-level의 복잡한 내용을 higher level의 간단한 모델에 숨기는 방식으로, 다른 수준의 구조를 표현하는 효과적인 방식입니다.

### Make the Common Case Fast

**Common case fast**하게 만든다는 것은 rare case를 최적화하는 것보다 성능을 향상시키는 것에 집중한다는 것을 의미합니다. **Common case**는 **rare case**보다 간단한 경우가 많기 때문에, 이 경우 성능을 향상시키는 것 또한 더 간단합니다. <br>
이 내용은 경험과 측정이 있어야 찾을 수 있는 common case를 무엇인지 확실히 하는 것이 중요함을 의미합니다.

### Performance via Parallelism

컴퓨팅의 성능을 계속해서 높이기 위한 방법으로, 컴퓨터는 operation을 parallel(병렬)하게 동작하기 위해 설계되어 왔습니다.

### Performance via Pipelining

**Pipelining**은 parallelism의 특정 방식으로, 일반적인 최적화 방식으로 널리 알려졌기 때문에 스스로 pipelining이라는 이름이 붙여졌습니다. <br>
한번의 동작은 여러 개의 명령으로 이루어지는데, 이 명령을 이전 동작의 완료를 기다리지 않고 계속해서 미리미리 처리하는 방식입니다.

### Performance via Prediction

허락보다는 용서가 쉽다는 말이 있듯이, 컴퓨터 구조에도 이 내용을 토대로 한 방식이 존재합니다. **Prediction**은 동작을 확실히 알고 처리하는 것보다 결과를 예측해서 처리하는 것이 일반적으로 빠르기 때문에 설계된 구조입니다. 만약 잘못 예측했더라도 이를 recover하는 것은 비용이 그리 비싸지 않고 prediction이 대체적으로 정확하다면, 이 방식은 매우 높은 성능 향상을 불러옵니다.

### Hierarchy of Memories

메모리는 빠르고, 크고, 저렴해야 합니다. 또, 메모리의 크기는 해결 가능한 problem의 크기 제한과 관련이 있습니다. 따라서 현대 컴퓨터의 비용은 메모리의 비용과 밀접한 관련이 있습니다. <br>
컴퓨터 구조 설계자들은 다른 특징의 메모리들을 계층적으로 배치하면 높은 성능을 기대할 수 있다는 것을 확인했습니다. 가장 빠르고, 작고 비싼 메모리를 hierarchy의 가장 위에 놓고, 그 반대의 메모리를 아래에 배치하는 방식입니다.

### Dependability via Redundancy

컴퓨터는 빨라야 할 뿐만 아니라 **dependable**해야 합니다. 물리적 장치는 **fail**이 발생할 수 있기 때문에, 우리는 시스템을 **dependable**하게 만들어서 **failure**가 발생하면 이를 감지할 수 있도록 해야 합니다.

---

## Abstraction

워드프로세서나 거대한 데이터베이스 시스템은 수백만 줄의 코드로 구성되고 여러 소프트웨어 라이브러리가 종속되어 있습니다. 하지만 컴퓨터의 하드웨어는 굉장히 low-level의 instruction만을 처리할 수 있습니다. 이렇게 간단한 명령만을 통해 복잡한 애플리케이션이 실행되려면 high-level operation을 해석하고 translate하는 여러 개의 레이어가 필요합니다. 이때 **abstraction**을 통해 이 구조를 간단하게 표현할 수 있습니다.

<img src="/computer_architecture/abstraction.png" width="50%">

위 abstraction에서 나와있듯이, application과 hardware 사이를 연결해주는 **systems software**가 하드웨어와 복잡한 애플리케이션 사이를 연결해주는 매개체가 됩니다. <br>
Systems software는 여러 종류가 조재하지만, 오늘날의 컴퓨터 시스템에서는 두 가지가 중심이 됩니다.

### Operating System

**Operating system**은 사용자의 프로그램과 하드웨어 사이를 조율합니다. I/O를 컨트롤하거나, storage와 memory를 할당하고, 여러 개의 애플리케이션을 컴퓨터가 동시에 다룰 수 있도록 도와줍니다. OS의 예시로는 Linux, iOS, Windows 등이 있습니다.

### Compiler

**Compiler**는 high-level language로 쓰여진 프로그램을 번역하는 주요 기능을 수행합니다. C, C++, Java와 같은 언어로 쓰여진 코드는 하드웨어가 직접 실행할 수 없으므로, compiler가 이를 읽고 하드웨어가 실행가능한 명령들로 변환합니다.

컴퓨터 하드웨어는 *on*과 *off*, 두 가지 신호만을 인식합니다. 이 두 가지 신호는 0과 1로 표현되고, 이를 **binary numbers**(이진수)라고 칭합니다. 0과 1과 같은 *letter*은 **binary digit** 또는 **bit**라고 명명합니다. <br>
컴퓨터는 우리들이 지시하는 **instruction**들로 명령을 수행합니다. Instruction은 컴퓨터가 이해할 수 있는 bit들로 이루어져 있습니다. 

> 예를 들어, 1000110010100000 <br>
> 위 비트는 컴퓨터에게 두 숫자를 더하라는 것을 의미합니다.

초기 프로그래머들은 이진수를 통해 컴퓨터와 소통하였습니다. 하지만 이진수를 이용한 소통은 사람이 직접 명령을 번역하는 과정이 필요했기 때문에 효율성이 떨어졌습니다. 따라서 의미있는 단어들을 이진수로 번역하는 프로그램을 개발하였는데, 초기에 개발한 이 프로그램을 **assembler**라고 불렀습니다. 이 프로그램은 사람이 알아볼 수 있는 단어들을 컴퓨터가 인식하는 이진수 instruction으로 번역하는 기능을 지녔습니다.

> 예를 들어, 프로그래머가 다음과 같은 텍스트를 작성한다면, <br>
> `add A,B` <br>
> assembler는 이를 다음으로 번역하게 됩니다. <br>
> 1000110010100000

이런 symbolic language는 여전히 사용되고 있는데, 이를 **assembly language**라고 말합니다. 반대로, 기계가 이해할 수 있는 binary language를 **machine language**라고 부릅니다.

하지만 이런 어셈블리어의 발전이 있더라도 과학자들의 실험이나 은행의 이체와 같은 복잡한 기능은 여전히 구현이 어려웠습니다. 어셈블리어는 프로그래머에게 컴퓨터가 실행해야 하는 모든 instruction을 라인마다 작성해야 했기 때문에, 프로그래머에게 컴퓨터와 같은 사고를 강요했습니다.

따라서 어셈블리어보다 더 'powerful'한 언어가 필요했습니다. 오늘날의 프로그래머들은 자신들의 생산성 증진을 위해 **high-level programming language**를 개발하고, 이를 번역하는 컴파일러를 개발하였습니다. 하이레벨 프로그래밍 언어는 여러 중요한 이점을 제공해 주었습니다.

1. 프로그래머가 자연어로 사고할 수 있도록 함
- 프로그램이 일반적인 텍스트처럼 보여지게 만들었습니다.
- 각 언어들이 의도한 사용법대로 디자인되기 때문에 생산성이 올라갔습니다.
    - Fortran은 과학적 계산에 사용되고, Cobol은 비즈니스 데이터 처리에 사용됩니다.
- 유체 시뮬레이션에 사용되는 언어처럼 domain-specific 언어가 존재합니다.

2. 프로그래머의 생산성 증진
- 자신의 생각을 표현하기 위해 작성해야 하는 코드 길이가 짧아졌습니다.
- 프로그램을 개발하는 데에 소요되는 시간이 줄어들었습니다.

3. 프로그램이 컴퓨터의 종류에 종속되지 않음
- 컴파일러와 어셈블리어는 어느 컴퓨터에서든 high-level language를 기계어로 번역할 수 있습니다.