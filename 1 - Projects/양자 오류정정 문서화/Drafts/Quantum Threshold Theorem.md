---
title: Quantum Threshold Theorem
aliases: [양자 임계값 정리, 임계값 정리, Quantum Threshold Theorem, Threshold Theorem, Fault-Tolerance Threshold, 결함 허용 임계값, Accuracy Threshold, Error Threshold]
type: concept
status: budding
domain: quantum-error-correction
tags:
  - qec/stabilizer
  - concept/fault-tolerance
  - qec/motivation
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Quantum Error Correction]]"
related:
  - "[[Fault-Tolerant Quantum Computation]]"
  - "[[Surface Code]]"
  - "[[Stabilizer Code]]"
  - "[[Code Distance]]"
  - "[[Logical Qubit]]"
  - "[[Magic State Distillation]]"
source:
  - "Aharonov and Ben-Or, Fault-tolerant quantum computation with constant error (1997), arXiv:quant-ph/9611025"
  - "Knill, Laflamme, Zurek, Resilient quantum computation, Science 279 (1998)"
  - "Nielsen and Chuang, Quantum Computation and Quantum Information, Ch. 10"
confidence: high
---

# Quantum Threshold Theorem

> 물리 게이트당 오류율 $p$가 어떤 임계값 $p_{\mathrm{th}}$보다 작기만 하면, 자원 오버헤드를 목표 정확도 $\epsilon$의 역수에 대한 폴리로그 수준으로만 늘려 임의로 긴 양자 계산을 임의로 정확하게 수행할 수 있다는 정리.

## 핵심
양자 임계값 정리는 결함 허용 양자컴퓨팅이 원리적으로 가능하다는 사실을 보증하는 존재 정리다. 핵심 주장은 한 문장으로 압축된다. 부품 하나하나가 완벽할 필요는 없고, 오류율이 충분히 작아 어떤 고정된 문턱 아래로만 내려가면 된다는 것이다. 이 문턱이 임계값 $p_{\mathrm{th}}$다. 물리 오류율 $p$가 $p < p_{\mathrm{th}}$를 만족하면 오류정정을 충분히 켜서 논리 오류율을 원하는 만큼 작게 만들 수 있고, 그 대가로 치르는 큐비트 수와 게이트 수는 견딜 만하게 천천히 늘어난다.

이 보증이 어떻게 가능한지는 부호 연접(concatenation) 논증이 가장 선명하게 보여 준다. 거리 작은 [[Stabilizer Code|안정자 부호]] 하나를 골라, 그 부호의 각 물리 큐비트를 다시 같은 부호로 한 겹 더 인코딩하는 일을 $L$번 반복한다. 한 겹을 씌울 때마다 논리 오류율이 제곱 꼴로 줄어든다는 점이 결정적이다. 한 단계 인코딩 뒤의 논리 오류율은 $p$에 비례하는 것이 아니라 대략 $p^2$에 비례하는데, 논리 오류가 일어나려면 단일 오류 하나로는 부족하고 부호가 막지 못하는 두 개의 독립 오류가 겹쳐야 하기 때문이다. 더 정확히는 한 단계마다 $p$가 $p_{\mathrm{th}}(p/p_{\mathrm{th}})^2$ 정도로 바뀌고, 이를 $L$번 반복하면 비율 $p/p_{\mathrm{th}}$가 거듭 제곱되어 다음과 같은 이중지수 억제가 나타난다.

$$ p_L \sim p_{\mathrm{th}} \left( \frac{p}{p_{\mathrm{th}}} \right)^{2^{L}} $$

여기서 $p < p_{\mathrm{th}}$이면 괄호 안의 비율이 1보다 작으므로, 지수 $2^L$이 단계 수 $L$에 따라 폭발적으로 커지면서 $p_L$이 무섭게 빠르게 0으로 향한다. 반대로 $p > p_{\mathrm{th}}$이면 같은 식에서 비율이 1보다 커서 연접을 거듭할수록 오히려 오류가 증폭되므로, 임계값은 억제와 증폭이 뒤집히는 정확한 경계로 자연스럽게 등장한다.

자원 쪽은 정반대로 점잖게 늘어난다. 한 겹마다 물리 큐비트 수가 상수 배 $C$만큼 곱해지므로 $L$겹의 총 오버헤드는 $C^L$이다. 목표 논리 오류율 $\epsilon$에 도달하는 데 필요한 단계 수 $L$은 위 이중지수 관계에서 $\log\log(1/\epsilon)$ 수준에 그치고, 그 결과 오버헤드 $C^L$은 $1/\epsilon$에 대한 폴리로그(polylog)로 정리된다. 길이 $T$짜리 회로를 신뢰성 있게 돌리려면 $\epsilon$을 $1/T$ 수준으로 잡아야 하므로, 오버헤드는 회로 크기 $T$에 대해서도 $\mathrm{polylog}(T)$로만 자란다. 정확도는 이중지수로 좋아지는데 비용은 다항보다 훨씬 느린 폴리로그로만 드는 이 비대칭이 정리의 힘이다.

## 흐름
```mermaid
flowchart TD
    A["물리 오류율 $$p$$"] --> B{"$$p < p_{\mathrm{th}}$$ ?"}
    B -- "예" --> C["연접 $$L$$겹 적용"]
    C --> D["논리 오류율 $$p_L \sim p_{\mathrm{th}}(p/p_{\mathrm{th}})^{2^{L}}$$ 이중지수 억제"]
    C --> E["오버헤드 $$C^{L} = \mathrm{polylog}(1/\epsilon)$$ 다항 이하 증가"]
    D --> F["임의 길이 계산을 임의 정확도로"]
    E --> F
    B -- "아니오" --> G["연접할수록 오류 증폭, 보증 없음"]
```

## 왜 중요한가
이 정리가 없었다면 양자컴퓨팅은 잡음 앞에서 무력했을 것이다. 결잃음과 게이트 부정확성은 회로가 길어질수록 누적되어 양자 상태를 망가뜨리고, 단순히 오류정정을 끼워 넣는 것만으로는 부족하다. 오류정정 회로 자체도 부정확한 게이트로 만들어지므로, 고치는 과정이 새로운 오류를 들여올 수 있기 때문이다. 임계값 정리는 이 자기 참조적 난점을 정면으로 다뤄, 정정 장치까지 결함이 있어도 물리 오류율만 문턱 아래이면 전체 계산이 안정된다는 점을 증명한다. 그래서 이 정리는 [[Fault-Tolerant Quantum Computation|결함 허용 양자컴퓨팅]]의 이론적 토대이며, 내결함성 게이트 구성과 결합해야 비로소 완전한 보증이 된다. 임계값은 회로가 안정 영역에 있는지 가르는 문턱을 주고, 내결함성 게이트는 그 문턱 아래에서 실제로 논리 연산을 흐트러뜨리지 않고 수행하는 방법을 준다.

임계값의 구체적인 수치는 보편 상수가 아니라 선택한 부호와 잡음 모형에 따라 달라진다. 초기의 연접 부호 분석은 매우 작은 값을 주었지만, 위상 부호 계열이 등장하면서 사정이 크게 나아졌다. 특히 [[Surface Code|표면 부호]]는 국소적인 이웃 큐비트끼리만 상호작용해도 신드롬을 뽑을 수 있고 디코딩이 잘 정의되어, 독립 잡음 모형에서 약 $1\%$ 안팎이라는 너그러운 임계값을 보인다. 이 값이 중요한 이유는 실험에서 도달 가능한 게이트 충실도의 범위 안에 들어오기 때문이다. 물리 오류율이 임계값 아래로만 내려오면, 표면 부호에서는 [[Code Distance|부호 거리]] $d$를 키우는 것만으로 논리 오류율이 거리에 대해 지수적으로 떨어지고, 이렇게 보호된 정보 한 덩어리가 한 개의 [[Logical Qubit|논리 큐비트]]가 된다. 임계값 정리는 바로 이 거리를 키우는 전략이 자원 측면에서 감당 가능하다는 것을 보장하는 상위의 약속이다.

## 연결
- [[Fault-Tolerant Quantum Computation]] 임계값 정리가 결함 허용 양자컴퓨팅의 가능성을 보증하는 이론적 토대
- [[Surface Code]] 약 $1\%$ 안팎의 너그러운 임계값을 주는 대표적 위상 부호이자 거리 확장으로 정리를 실현하는 구현
- [[Stabilizer Code]] 연접 논증과 신드롬 기반 오류정정이 작동하는 부호 틀
- [[Code Distance]] 임계값 아래에서 논리 오류율을 지수적으로 억제하는 핵심 자원
- [[Logical Qubit]] 임계값 정리가 임의 정확도로 보호하겠다고 약속하는 정보 단위
- [[Magic State Distillation]] 임계값 정리의 점근적 효율과 증류 공장의 실제 자원 오버헤드 사이에 간극을 만드는 내결함성 구성 요소
- [[Quantum Error Correction]] 임계값 정리가 의의를 부여하는 상위 분야
