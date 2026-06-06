---
title: Holevo Bound
aliases: [홀레보 한계, 홀레보 정리, Holevo's Theorem, 홀레보 경계]
type: concept
status: budding
domain: quantum-information-theory
tags:
  - concept/measurement
  - concept/state
  - math/linear-algebra
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Quantum Information Theory]]"
related:
  - "[[Superdense Coding]]"
  - "[[Qubit]]"
  - "[[Density Matrix]]"
  - "[[Quantum Measurement]]"
source:
  - "Holevo (1973), Problems of Information Transmission 9, 177"
  - "Nielsen, Chuang, Quantum Computation and Quantum Information, 12.1.1"
confidence: high
---

# Holevo Bound

> 양자 상태를 측정해 얻을 수 있는 고전 정보의 양에 걸리는 상한으로, $n$개 큐비트가 운반할 수 있는 고전 정보가 $n$비트를 넘지 못함을 보장한다.

## 핵심
홀레보 한계는 다음과 같은 통신 상황을 다룬다. 앨리스가 확률 $p_x$로 고전 기호 $x$를 고른 뒤, 그 기호를 양자 상태 $\rho_x$에 실어 밥에게 보낸다. 밥은 받은 상태를 한 번 측정해 측정 결과 $Y$를 얻고, 이로부터 원래 기호 $X$를 추정한다. 밥이 어떤 측정을 쓰든 그가 $X$에 대해 알게 되는 정보량, 즉 상호정보 $I(X;Y)$에는 측정 방식과 무관한 천장이 존재한다. 이 천장이 홀레보 한계이며, 그 값을 홀레보 정보 $\chi$라 부른다.

$$ I(X;Y) \le \chi = S\!\left( \sum_x p_x\, \rho_x \right) - \sum_x p_x\, S(\rho_x) $$

여기서 $S(\rho) = -\operatorname{Tr}(\rho \log_2 \rho)$는 [[Von Neumann Entropy|폰 노이만 엔트로피]]다. 앞항은 밥이 받는 평균 상태 $\rho = \sum_x p_x \rho_x$의 엔트로피이고, 뒷항은 각 신호 상태가 가진 엔트로피의 평균이다. 두 항의 차이는 신호들이 서로 얼마나 잘 구별되는지를 재는 양으로, 신호 [[Density Matrix|밀도행렬]]들이 직교에 가까울수록 커진다.

### 차원 상한과 큐비트 한 개의 한계
폰 노이만 엔트로피는 힐베르트 공간의 차원 $d$에 대해 $S(\rho) \le \log_2 d$로 묶이고, 뒷항은 음이 아니므로 홀레보 정보 자체가 다음으로 제한된다.

$$ \chi \le S(\rho) \le \log_2 d $$

[[Qubit|큐비트]] $n$개로 이루어진 계는 차원이 $d = 2^n$이므로 $\log_2 d = n$이다. 따라서 어떤 인코딩과 어떤 [[Quantum Measurement|측정]]을 쓰더라도 $n$개 큐비트에서 추출 가능한 고전 정보는 $n$비트를 넘을 수 없다. 큐비트 한 개라면 $d = 2$이므로 정확히 한 비트가 한계다. 큐비트의 상태가 연속 매개변수 $\alpha, \beta$로 무한히 많은 값을 담는 듯 보여도, 측정을 통해 고전적으로 꺼낼 수 있는 정보는 단 한 비트로 봉인된다.

### 등호 조건
홀레보 한계가 등호로 포화하려면 신호 상태들이 서로 직교해야 한다. 직교하는 신호는 사영측정으로 완벽히 구별되어 $I(X;Y)$가 $\chi$에 도달하고, 신호가 모두 순수하고 직교하면 추출 정보가 $\log_2 d$에 이른다. 신호들이 겹치면 구별 오류가 생겨 정보가 한계 아래로 떨어진다.

## 구조
```mermaid
flowchart LR
    X["고전 기호 $$X$$, 분포 $$p_x$$"] --> ENC["인코딩 $$x \mapsto \rho_x$$"]
    ENC --> CH["양자 채널 전송"]
    CH --> MEAS["밥의 측정 (임의 POVM)"]
    MEAS --> Y["측정 결과 $$Y$$"]
    Y --> EST["추정 $$\hat{X}$$"]
    BND["상한 $$I(X;Y) \le \chi \le \log_2 d$$"] -.제약.- MEAS
```

## 왜 중요한가
홀레보 한계는 양자계가 고전 정보의 저장과 전송에서 차원 이상의 마법을 부리지 못한다는 사실을 못 박는 정보론의 기초 정리다. 1973년 홀레보가 증명했으며, 양자 채널의 고전 용량을 다루는 모든 논의의 출발점이 된다.

특히 이 한계는 [[Superdense Coding|초고밀도 부호화]]의 겉보기 역설을 해소한다. 초고밀도 부호화는 큐비트 하나를 보내고 두 고전 비트를 전달하지만, 이것이 큐비트 하나가 두 비트를 담는다는 의미는 아니다. 홀레보 한계는 큐비트 하나의 고전 정보를 한 비트로 묶으며, 초고밀도 부호화가 추가로 소모하는 자원은 사전에 분배해 둔 [[Quantum Entanglement|얽힘]] 큐비트다. 전송 큐비트 한 개와 얽힘 큐비트 한 개를 합쳐 차원이 $2^2$인 계가 동원되므로 두 비트 전달은 $\log_2 4 = 2$라는 한계와 정확히 일치한다. 즉 홀레보 한계는 양자 통신에서 자원 회계를 강제하는 심판 역할을 한다.

나아가 이 결과는 양자 상태에 고전 정보가 어떻게 인코딩되고 디코딩되는지에 대한 직관을 교정한다. 양자 상태는 무한한 연속 정보를 품은 듯 보이지만, 측정이라는 비가역적 추출 과정을 거치는 순간 꺼낼 수 있는 고전 정보는 엄격히 유한하다. 이 관점은 [[Accessible Information|접근 가능 정보]]와 양자 채널 용량 정리, 그리고 양자 [[No-Cloning Theorem|복제 불가 정리]]가 통신에 끼치는 제약을 이해하는 토대가 된다.

## 연결
- [[Superdense Coding]] 큐비트 하나로 두 비트를 보내는 프로토콜이 이 한계와 모순되지 않음을 자원 회계로 설명하는 직접 응용
- [[Qubit]] 차원 $d = 2^n$ 계산의 기본 단위로, 큐비트 한 개의 고전 정보 한계가 한 비트임을 규정
- [[Density Matrix]] 홀레보 정보를 구성하는 신호 상태와 평균 상태가 모두 밀도행렬로 표현됨
- [[Quantum Measurement]] 어떤 측정을 택하든 상호정보가 이 상한 아래에 머문다는 보편적 제약의 대상
- [[Von Neumann Entropy]] 홀레보 정보 $\chi$를 정의하는 엔트로피 척도(향후 작성)
- [[Quantum Entanglement]] 초고밀도 부호화에서 한계를 우회하지 않고 보충하는 추가 자원
- [[Accessible Information]] 측정으로 실제 추출 가능한 정보량으로, 홀레보 정보가 그 상한을 이룸(향후 작성)
