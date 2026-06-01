---
title: Partial Trace
aliases: [부분 대각합, 부분 대각합 연산, 부분 트레이스, Partial Trace, Tr_B, 환원 밀도행렬 연산]
type: concept
status: budding
domain: foundations
tags:
  - concept/formalism
  - concept/composite-system
  - concept/mixed-state
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
related:
  - "[[Density Matrix]]"
  - "[[Quantum Entanglement]]"
  - "[[Tensor Product]]"
  - "[[Quantum Decoherence]]"
confidence: high
---

# Partial Trace

> 복합계의 밀도 행렬에서 관심 없는 부분계의 자유도를 합산해 평균해 내고, 보고자 하는 부분계만의 환원 밀도행렬을 얻는 선형 연산이다.

## 핵심
계 $AB$가 $\mathcal{H}_A \otimes \mathcal{H}_B$에 살고 그 상태가 [[Density Matrix|밀도 행렬]] $\rho_{AB}$로 주어졌다고 하자. 우리가 $A$만 관측하고 $B$에는 손댈 수 없을 때, $A$가 보이는 모든 측정 통계를 빠짐없이 재현하는 객체가 $A$의 환원 밀도행렬 $\rho_A$다. 이를 얻는 연산이 $B$에 대한 부분 대각합으로, $B$의 지표만 따로 대각합을 취해 사라지게 한다.

$$ \rho_A = \mathrm{Tr}_B(\rho_{AB}) $$

전체 대각합 $\mathrm{Tr}$이 두 지표를 모두 합산해 스칼라 하나를 내놓는 것과 달리, 부분 대각합은 $B$의 지표만 합산하므로 결과는 여전히 $A$ 위의 연산자다. 정의를 성분으로 적으면 다음과 같다.

$$ \big(\rho_A\big)_{ii'} = \sum_{j} \langle i,j \rvert \, \rho_{AB} \, \lvert i',j \rangle $$

여기서 $\{\lvert i \rangle\}$는 $\mathcal{H}_A$의 기저, $\{\lvert j \rangle\}$는 $\mathcal{H}_B$의 기저이며, $j$가 같은 항끼리 묶어 더한다. 좌표에 의존하지 않는 정의는 단항식 $\lvert a_1 \rangle\langle a_2 \rvert \otimes \lvert b_1 \rangle\langle b_2 \rvert$ 위에서 다음처럼 작용하고 선형으로 확장하는 것이다.

$$ \mathrm{Tr}_B\big( \lvert a_1 \rangle\langle a_2 \rvert \otimes \lvert b_1 \rangle\langle b_2 \rvert \big) = \lvert a_1 \rangle\langle a_2 \rvert \cdot \langle b_2 \vert b_1 \rangle $$

즉 $A$ 쪽 연산자는 그대로 두고 $B$ 쪽 연산자는 자기 대각합 $\langle b_2 \vert b_1 \rangle$로 수축시킨다.

### 잘 정의된 연산임을 보장하는 성질
부분 대각합은 단순한 계산 기교가 아니라, 부분계 기술을 유일하게 결정하는 연산이다. 핵심 성질은 임의의 $A$ 측 관측가능량 $M_A$에 대해 다음 일관성이 성립한다는 것이다.

$$ \mathrm{Tr}\big( M_A \, \rho_A \big) = \mathrm{Tr}\big( (M_A \otimes I_B)\, \rho_{AB} \big) $$

이 등식은 $A$에만 작용하는 어떤 측정을 하더라도 $\rho_A$로 계산한 기댓값이 전체 $\rho_{AB}$로 계산한 값과 항상 일치함을 뜻한다. 이 조건을 만족하는 $A$ 위의 연산자는 유일하며, 그것이 곧 $\mathrm{Tr}_B(\rho_{AB})$다. 따라서 부분 대각합은 부분계를 기술하는 유일하게 옳은 방법이다. 또한 부분 대각합은 밀도 행렬을 다시 밀도 행렬로 보내는데, 결과 $\rho_A$는 에르미트성, 양의 준정부호, 대각합 1을 모두 보존한다.

## 흐름
```mermaid
flowchart TD
    AB["복합계 상태 $$\rho_{AB}$$ on $$\mathcal{H}_A \otimes \mathcal{H}_B$$"] -- "$$\mathrm{Tr}_B$$ ($$B$$ 합산)" --> RA["부분계 $$A$$의 환원 밀도행렬 $$\rho_A$$"]
    AB -- "$$\mathrm{Tr}_A$$ ($$A$$ 합산)" --> RB["부분계 $$B$$의 환원 밀도행렬 $$\rho_B$$"]
    RA --> CHK{"$$\mathrm{Tr}(\rho_A^2) = 1$$?"}
    CHK -- "예" --> PROD["곱 상태, $$A$$는 순수"]
    CHK -- "아니오" --> ENT["얽힘, $$A$$는 혼합"]
```

### 벨 상태 예시
가장 분명한 예는 [[Bell States|벨 상태]] $\lvert \Phi^{+} \rangle = \tfrac{1}{\sqrt{2}}(\lvert 00 \rangle + \lvert 11 \rangle)$이다. 전체는 순수 상태 $\rho_{AB} = \lvert \Phi^{+} \rangle\langle \Phi^{+} \rvert$이지만, $B$를 부분 대각합으로 지우면 $A$는 정보가 전혀 없는 최대 혼합 상태가 된다.

$$ \rho_A = \mathrm{Tr}_B\big( \lvert \Phi^{+} \rangle\langle \Phi^{+} \rvert \big) = \frac{1}{2}\big( \lvert 0 \rangle\langle 0 \rvert + \lvert 1 \rangle\langle 1 \rvert \big) = \frac{I}{2} $$

전체에 대한 완전한 지식이 있어도 부분에 대한 지식은 사라지는 이 역설적 결과가 [[Quantum Entanglement|얽힘]]의 정보론적 지문이며, 부분 대각합은 그것을 드러내는 도구다.

## 왜 중요한가
부분 대각합은 닫힌 전체계의 형식과 우리가 실제로 접근하는 부분계의 형식을 잇는 다리다. 현실의 어떤 양자계도 환경과 [[Tensor Product|텐서곱]]으로 결합한 더 큰 계의 한 조각일 뿐이므로, 관측 가능한 통계를 얻으려면 나머지 자유도를 부분 대각합으로 평균해야 한다. 이 연산이 있어야 [[Quantum Decoherence|결어긋남]]을 환경을 추적해 낸 결과로 정량화할 수 있고, 양자 채널과 노이즈 모형을 환경과의 상호작용을 부분 대각합으로 닫는 방식으로 유도할 수 있으며, 얽힘 엔트로피 $S(\rho_A) = -\mathrm{Tr}(\rho_A \log_2 \rho_A)$로 얽힘의 양을 측정할 수 있다. 부분 대각합이 없으면 부분계만 보는 모든 양자정보 처리는 기술할 언어를 잃는다.

## 연결
- [[Density Matrix]] 부분 대각합의 입력과 출력이 모두 밀도 행렬이며, 환원 밀도행렬을 생성하는 연산
- [[Quantum Entanglement]] 부분 대각합으로 얻은 환원 밀도행렬의 혼합도가 곧 얽힘의 척도
- [[Tensor Product]] 부분 대각합이 작용하는 복합계 상태 공간을 만드는 결합 연산이자 그 역방향에 해당
- [[Quantum Decoherence]] 환경 자유도를 부분 대각합으로 추적해 내면 결맞음 상실이 비대각 항의 소멸로 나타남
