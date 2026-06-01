---
title: Surface Code
aliases: [표면 부호, 표면 코드, 서피스 코드, Surface Codes]
type: concept
status: budding
domain: quantum-error-correction
tags:
  - qec/surface-code
  - qec/stabilizer
  - hardware/superconducting
  - concept/fault-tolerance
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Quantum Error Correction]]"
related:
  - "[[양자 하드웨어 로드맵 추적]]"
  - "[[Stabilizer Code]]"
  - "[[Pauli Matrices]]"
  - "[[Quantum Decoherence]]"
  - "[[Qubit]]"
  - "[[Superconducting Qubit]]"
source:
  - "Fowler et al., Surface codes: Towards practical large-scale quantum computation, Phys. Rev. A 86, 032324 (2012), DOI: 10.1103/PhysRevA.86.032324"
  - "Kitaev, Fault-tolerant quantum computation by anyons, Ann. Phys. 303, 2003"
confidence: high
---

# Surface Code

> 큐비트를 2차원 격자에 배치하고 국소적인 이웃 안정자 측정만으로 오류를 진단하는 위상적 양자 오류정정 부호로, 높은 임계값과 평면 구조 덕분에 내결함성 양자컴퓨터의 가장 유력한 후보로 꼽힌다.

## 핵심
Surface Code는 [[Stabilizer Code|안정자 부호]]의 한 종류로, 데이터 큐비트를 정사각 격자의 변에 두고 그 사이사이에 측정 보조(ancilla) 큐비트를 배치한다. 각 측정 보조 큐비트는 이웃한 데이터 큐비트들에 대해 두 종류의 안정자 연산자를 반복 측정한다. 하나는 [[Pauli Matrices|파울리]] $X$ 연산자의 곱으로 정의되는 정점(vertex) 연산자 $A_v$이고, 다른 하나는 파울리 $Z$ 연산자의 곱으로 정의되는 플라켓(plaquette) 연산자 $B_p$이다. 격자가 매끈할 때 이 두 종류의 안정자는 다음과 같이 표기한다.

$$ A_v = \prod_{i \in v} X_i, \qquad B_p = \prod_{i \in p} Z_i $$

모든 안정자는 서로 가환이므로 동시에 측정할 수 있고, 측정 결과(신드롬)는 데이터 큐비트를 직접 건드리지 않은 채 오류의 흔적만 드러낸다. 오류가 없으면 모든 안정자의 고유값은 $+1$이고, 어딘가에서 $-1$이 관측되면 그 부근에 오류가 일어났다는 신호가 된다. 단일 비트반전 오류나 위상반전 오류는 격자 위에서 끝점 쌍의 형태로 신드롬을 남기며, 복호기는 이 끝점들을 짝지어 가장 그럴듯한 오류 사슬을 추정한다. 이 짝짓기에는 최소 가중치 완전 매칭 같은 [[복호 알고리즘]]이 쓰인다.

부호의 성능을 지배하는 핵심 매개변수는 부호 거리 $d$이다. 거리는 논리 연산자를 이루는 가장 짧은 오류 사슬의 길이를 뜻하며, 거리 $d$인 부호는 $\lfloor (d-1)/2 \rfloor$개까지의 오류를 정정한다. 물리 오류율 $p$가 임계값 $p_{\mathrm{th}}$보다 작은 영역에서는 거리를 키울수록 논리 오류율 $p_L$이 지수적으로 떨어진다.

$$ p_L \sim \left( \frac{p}{p_{\mathrm{th}}} \right)^{\lfloor (d+1)/2 \rfloor} $$

Surface Code가 주목받는 이유 중 하나가 바로 이 임계값으로, 약 $1\%$ 안팎이라는 비교적 너그러운 값을 가진다. 게이트 오류율을 이 수준 아래로만 떨어뜨리면 거리를 늘리는 것만으로 임의로 낮은 논리 오류율에 도달할 수 있다.

## 구조
다음은 신드롬 추출에서 논리 큐비트 보호까지 이어지는 흐름이다.

```mermaid
flowchart TD
    D["데이터 큐비트<br/>(2차원 격자)"] --> S["안정자 측정<br/>($$A_v = \prod X_i,\ B_p = \prod Z_i$$)"]
    S --> Y["신드롬 추출<br/>(고유값 $$-1$$ 위치)"]
    Y --> M["복호기<br/>(최소 가중치 매칭)"]
    M --> R["오류 사슬 추정과 정정"]
    R --> L["보호된 논리 큐비트<br/>(거리 $$d$$, $$p_L \downarrow$$)"]
```

## 왜 중요한가
물리 큐비트는 [[Quantum Decoherence|결어긋남]]과 게이트 불완전성 탓에 본질적으로 잡음에 취약하므로, 쓸모 있는 계산을 하려면 다수의 잡음 섞인 물리 큐비트를 묶어 하나의 안정적인 [[Logical Qubit|논리 큐비트]]로 추상화해야 한다. Surface Code는 이 변환을 가장 현실적으로 해내는 부호로 평가받는데, 그 이유는 두 가지이다. 첫째, 안정자가 모두 격자 위 최근접 이웃끼리의 국소 측정으로 구성되어 [[Qubit|큐비트]]를 평면에 배치하는 [[Superconducting Qubit|초전도]] 같은 하드웨어와 자연스럽게 맞물린다. 전 연결성을 요구하지 않으므로 칩 설계 부담이 작다. 둘째, 약 $1\%$의 높은 임계값 덕분에 현재 도달 가능한 게이트 충실도로도 임계값 아래 동작을 노릴 수 있다.

이 때문에 Surface Code는 [[양자 하드웨어 로드맵 추적|하드웨어 로드맵]]에서 단순한 물리 큐비트 수보다 더 중요한 진전 신호로 다뤄진다. 거리 $d$를 키울 때 논리 오류율이 실제로 내려가는지, 즉 임계값 아래에서 동작하는지가 내결함성 양자컴퓨터로 가는 사다리에서 결정적인 분기점이기 때문이다. 다만 대가도 크다. 하나의 고품질 논리 큐비트를 만드는 데 수백에서 수천 개의 물리 큐비트가 필요하므로, [[Shor's Algorithm|쇼어 알고리즘]]으로 RSA를 깨는 규모의 [[Cryptographically Relevant Quantum Computer|CRQC]]에는 막대한 물리 자원이 요구된다.

## 연결
- [[양자 하드웨어 로드맵 추적]] 표면 부호의 거리 확장과 임계값 돌파를 논리 큐비트 진전의 핵심 신호로 받아 CRQC 시점 추정에 반영하는 책임 영역
- [[Stabilizer Code]] 표면 부호가 속한 상위 부호 계열로, 안정자 형식론의 일반 틀을 제공
- [[Superconducting Qubit]] 평면 격자 위 최근접 이웃 배치라는 표면 부호의 요구와 자연스럽게 맞물리는 대표적 물리 큐비트 실현
- [[Pauli Matrices]] 안정자 연산자 $A_v$와 $B_p$를 구성하는 파울리 $X$, $Z$의 곱의 기본 단위
- [[Quantum Decoherence]] 오류정정이 맞서 싸우는 잡음의 근본 원인이자 부호가 필요한 이유
- [[Qubit]] 데이터 큐비트와 측정 보조 큐비트로 격자를 채우는 기본 단위
- [[Logical Qubit]] 다수의 물리 큐비트를 묶어 표면 부호가 보호해 내는 추상화 대상(작성 예정)
- [[복호 알고리즘]] 신드롬에서 오류 사슬을 추정하는 최소 가중치 매칭 등의 복호 절차(작성 예정)
