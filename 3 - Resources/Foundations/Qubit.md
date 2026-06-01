---
title: Qubit
aliases: [큐비트, 양자비트, Quantum Bit]
type: concept
status: budding
domain: foundations
tags:
  - concept/qubit
  - concept/state
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
related:
  - "[[Quantum Superposition]]"
  - "[[Bloch Sphere]]"
  - "[[Dirac Notation]]"
  - "[[Density Matrix]]"
  - "[[Photonic Qubit]]"
  - "[[Superconducting Qubit]]"
confidence: high
---

# Qubit

> 두 개의 직교 기저 상태가 만드는 2차원 복소 힐베르트 공간 위의 단위 벡터로 표현되는, 양자정보의 최소 단위인 2준위 양자계다.

## 핵심
큐비트는 고전 정보의 비트에 대응하는 양자 정보의 기본 단위다. 물리적으로는 2준위 양자계, 즉 구별 가능한 두 상태만을 본질적 자유도로 갖는 계다. 이 두 상태를 계산 기저(computational basis)로 잡아 $\lvert 0 \rangle$과 $\lvert 1 \rangle$로 적는다. 두 기저는 서로 직교하며($\langle 0 \vert 1 \rangle = 0$) 정규화되어 있어, 2차원 복소 힐베르트 공간의 정규직교 기저를 이룬다. 표기는 [[Dirac Notation]]의 켓(ket) 표기를 따른다.

고전 비트가 0 또는 1 둘 중 하나만 갖는 것과 달리, 큐비트의 일반 상태는 두 기저의 [[Quantum Superposition|중첩]]으로 쓰인다.

$$ \lvert \psi \rangle = \alpha \lvert 0 \rangle + \beta \lvert 1 \rangle $$

여기서 진폭 $\alpha, \beta$는 복소수이며($\alpha, \beta \in \mathbb{C}$), 상태 벡터의 정규화 조건은 다음과 같다.

$$ \lvert \alpha \rvert^2 + \lvert \beta \rvert^2 = 1 $$

이 조건은 측정 시 $\lvert 0 \rangle$이 나올 확률 $\lvert \alpha \rvert^2$와 $\lvert 1 \rangle$이 나올 확률 $\lvert \beta \rvert^2$의 합이 1이라는 보른 규칙(Born rule)의 요청에서 나온다.

측정 전과 측정 후의 정보량이 크게 다르다는 점이 큐비트의 핵심이다. 측정 전 큐비트는 정규화된 복소 진폭 쌍으로 기술되는 연속적인 상태공간 위의 한 점이며, 원리상 무한히 많은 서로 다른 상태가 존재한다. 그러나 계산 기저로 한 번 측정하면 그 결과는 0 또는 1이라는 단 하나의 고전 비트로 환원되고, 상태는 측정된 기저 상태로 붕괴한다. 즉 큐비트가 담는 양자 상태는 연속적이지만, 한 번의 측정으로 추출할 수 있는 고전 정보는 정확히 1비트다. 풍부한 연속 자유도는 측정으로 직접 읽어내는 양이 아니라, 게이트 연산과 간섭을 통해 계산 자원으로 활용된다.

위상 자유도는 두 종류로 나뉘며 물리적 의미가 다르다. 상태 전체에 곱해지는 전역 위상(global phase) $e^{i\gamma}$는 어떤 측정으로도 검출되지 않는다. $\lvert \psi \rangle$와 $e^{i\gamma} \lvert \psi \rangle$는 모든 관측가능량에 대해 동일한 통계를 주므로 같은 물리 상태를 가리킨다. 반면 두 진폭 사이의 상대 위상(relative phase)은 물리적으로 실재한다.

$$ \lvert \psi \rangle = \cos\frac{\theta}{2} \lvert 0 \rangle + e^{i\varphi} \sin\frac{\theta}{2} \lvert 1 \rangle $$

전역 위상을 떼어내고 정규화를 적용하면 큐비트의 순수 상태는 위와 같이 두 실수 매개변수 $\theta$와 $\varphi$만으로 적힌다. 상대 위상 $\varphi$는 기저를 바꿔 측정할 때 간섭 무늬로 드러나며, 이 표현은 자연스럽게 [[Bloch Sphere|블로흐 구]] 위의 한 점으로 시각화된다. 혼합 상태나 노이즈가 섞인 경우의 일반적 기술은 [[Density Matrix|밀도 행렬]]로 넘어간다.

큐비트는 추상적 정보 단위이고, 이를 떠받치는 물리적 실현은 여러 갈래다. 초전도 회로를 쓰는 [[Superconducting Qubit]], 가둔 이온의 내부 준위를 쓰는 [[Trapped-Ion Qubit]], 광자의 편광이나 경로를 쓰는 [[Photonic Qubit]]가 대표적이다. 어떤 물리계를 쓰든 본질적 자유도가 2준위 구조로 환원되면 동일한 큐비트 추상으로 다룬다.

## 왜 중요한가
큐비트는 양자컴퓨팅과 양자정보이론 전체가 올라서는 토대다. 중첩과 상대 위상이라는 자유도가 있기에 양자 게이트가 간섭을 일으킬 수 있고, 그 간섭이 고전 계산으로 흉내 내기 어려운 알고리즘적 이득의 원천이 된다. 여러 큐비트를 결합하면 상태공간 차원이 $2^n$으로 지수적으로 커지고, 이 큰 공간과 얽힘이 결합해 양자 우월성의 가능성을 연다. 동시에 측정이 상태를 붕괴시키고 1비트만 내어준다는 제약은 [[No-Cloning Theorem|복제 불가 정리]]와 함께 양자암호의 안전성 근거가 되기도 한다. 큐비트의 정의를 정확히 잡는 일은 이후의 게이트, 얽힘, 오류정정, 알고리즘 논의가 모두 의존하는 출발점이다.

## 연결
- [[Quantum Superposition]] 큐비트의 일반 상태를 만드는 기저 중첩 원리이자 핵심 자유도
- [[Bloch Sphere]] 전역 위상을 제거한 단일 큐비트 순수 상태를 구면 위 한 점으로 시각화하는 기하 표현
- [[Dirac Notation]] $\lvert 0 \rangle$, $\lvert 1 \rangle$ 같은 상태와 진폭을 적는 표기 규약
- [[Density Matrix]] 혼합 상태와 노이즈까지 포함하는 큐비트 상태의 일반적 기술 형식
- [[Superconducting Qubit]] 추상적 큐비트를 초전도 회로로 실현한 대표적 물리 구현
