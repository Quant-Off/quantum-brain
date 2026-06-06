---
title: Amplitude Amplification
aliases: [진폭 증폭, 振幅증폭, Amplitude Amplification]
type: concept
status: budding
domain: quantum-algorithms
tags:
  - algorithm/grover
  - concept/superposition
  - concept/measurement
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Quantum Algorithms]]"
related:
  - "[[Grover's Algorithm]]"
  - "[[Grover Diffusion Operator]]"
  - "[[Quantum Measurement]]"
  - "[[Quantum Superposition]]"
source:
  - "G. Brassard, P. Høyer, M. Mosca, A. Tapp, Quantum Amplitude Amplification and Estimation, AMS Contemporary Mathematics 305 (2002)"
confidence: high
---

# Amplitude Amplification

> 임의의 준비 절차가 만든 상태에서 원하는 결과의 진폭을 두 반사의 반복으로 키워, 측정 성공 확률을 제곱근만큼 빠르게 끌어올리는 기법.

## 핵심
진폭 증폭은 [[Grover's Algorithm|그로버 알고리즘]]의 동력을 일반화한 것이다. 그로버가 균등 [[Quantum Superposition|중첩]]에서 출발해 단 하나의 표시된 해를 찾는 특수한 경우라면, 진폭 증폭은 출발 상태와 성공 판정 조건을 모두 임의로 둔다. 어떤 양자 절차 $\mathcal{A}$가 초기 상태 $\lvert 0 \rangle$에 작용해

$$ \mathcal{A} \lvert 0 \rangle = \lvert \psi \rangle = \sin\theta \, \lvert \psi_{\text{good}} \rangle + \cos\theta \, \lvert \psi_{\text{bad}} \rangle $$

를 만든다고 하자. 여기서 $\lvert \psi_{\text{good}} \rangle$는 성공으로 판정되는 부분공간으로 사영한 정규화 상태이고, $\lvert \psi_{\text{bad}} \rangle$는 실패 부분공간이다. 이때 한 번의 측정으로 성공을 얻을 확률은 $p = \sin^2\theta$다. 고전적으로 성공 확률을 보장 수준까지 끌어올리려면 $\mathcal{A}$를 평균 $1/p$번쯤 반복해야 한다. 진폭 증폭은 같은 일을 약 $1/\sqrt{p}$번의 $\mathcal{A}$ 호출로 해낸다.

핵심 장치는 두 종류의 반사를 합성한 연산자다. 첫째는 성공 부분공간의 위상을 뒤집는 오라클 반사

$$ S_{\chi} = I - 2 \lvert \psi_{\text{good}} \rangle \langle \psi_{\text{good}} \rvert $$

로, 성공 조건을 판정하는 술어 $\chi$를 이용해 좋은 상태의 부호만 바꾼다. 둘째는 출발 상태 $\lvert \psi \rangle$를 기준으로 한 반사

$$ S_0' = \mathcal{A} \, (2 \lvert 0 \rangle \langle 0 \rvert - I) \, \mathcal{A}^{-1} = 2 \lvert \psi \rangle \langle \psi \rvert - I $$

다. 이 둘을 병합한

$$ Q = -\, S_0' \, S_{\chi} $$

가 진폭 증폭의 한 반복을 이룬다. 그로버의 [[Grover Diffusion Operator|확산 연산자]]는 출발 상태가 균등 중첩 $\lvert s \rangle$인 특수한 형태의 $S_0'$에 해당한다. 진폭 증폭은 이를 임의의 $\mathcal{A}$로 교체했을 뿐, 골격은 동일하다.

기하학적으로 두 반사의 합성은 회전이다. 상태는 $\lvert \psi_{\text{good}} \rangle$과 $\lvert \psi_{\text{bad}} \rangle$가 펼치는 2차원 평면 안에 갇혀 있고, $Q$를 한 번 적용할 때마다 그 평면에서 각도 $2\theta$만큼 회전한다. 따라서 $Q$를 $k$번 적용하면 상태는

$$ Q^k \lvert \psi \rangle = \sin\big((2k+1)\theta\big) \, \lvert \psi_{\text{good}} \rangle + \cos\big((2k+1)\theta\big) \, \lvert \psi_{\text{bad}} \rangle $$

가 된다. 성공 진폭 $\sin\big((2k+1)\theta\big)$가 $1$에 가장 가까워지도록 반복 횟수를 고르면

$$ k \approx \frac{\pi}{4\theta} \approx \frac{\pi}{4} \frac{1}{\sqrt{p}} $$

이고, 여기서 $O(1/\sqrt{p})$의 가속이 나온다. 그로버는 표시된 해가 하나일 때 $\sin\theta = 1/\sqrt{N}$이므로 $p = 1/N$이 되어, 이 일반식이 $\frac{\pi}{4}\sqrt{N}$이라는 익숙한 결과로 환원된다.

회전이라는 성질은 측정 시점을 신중히 골라야 함을 뜻한다. 최적 횟수를 지나치면 진폭이 다시 줄어들기 때문이다. 출발 성공 확률 $p$를 미리 모르면 정확한 $k$를 정할 수 없는데, 이때는 반복 한계를 무작위로 키워 가며 시도하는 방식이나, $\theta$ 자체를 추정하는 진폭 추정(amplitude estimation)으로 대응한다. 추정으로 얻은 성공 확률은 [[Quantum Measurement|측정]] 결과의 통계를 직접 다루지 않고도 $p$를 알려 주므로, 몬테카를로 계산의 양자 가속에 쓰인다.

## 구조
```mermaid
flowchart TD
    A["$$\lvert 0 \rangle$$"] -->|"$$\mathcal{A}$$"| B["$$\lvert \psi \rangle = \sin\theta\,\lvert \psi_{\text{good}} \rangle + \cos\theta\,\lvert \psi_{\text{bad}} \rangle$$"]
    B --> C["$$S_{\chi}$$: 성공 부분공간 위상 반전"]
    C --> D["$$S_0' = 2\lvert \psi \rangle\langle \psi \rvert - I$$: 출발 상태 기준 반사"]
    D -->|"$$Q = -S_0' S_{\chi}$$ 를 약 $$\tfrac{\pi}{4\sqrt{p}}$$회 반복"| C
    D --> E["측정: 성공을 높은 확률로 관측"]
```

## 왜 중요한가
진폭 증폭은 그로버를 하나의 알고리즘에서 재사용 가능한 부품으로 끌어올린다. 어떤 양자 절차든 성공과 실패를 가르는 술어만 정의할 수 있으면, 그 절차를 통째로 $\mathcal{A}$로 감싸 성공 확률을 제곱근 가속으로 증폭할 수 있다. 덕분에 그로버 탐색은 물론, 충돌 찾기, 평균과 최솟값 추정, 그래프 문제 등 다양한 양자 알고리즘이 진폭 증폭을 공통 골격으로 공유한다. [[MOC - Quantum Algorithms|양자 알고리즘 지도]]가 진폭 증폭을 개별 알고리즘이 아니라 핵심 서브루틴으로 분류하는 이유가 여기에 있다.

특히 두 가지 일반화가 실용적 가치를 키운다. 첫째, 출발 분포를 균등 중첩에 묶지 않으므로, 사전 지식으로 성공 후보에 미리 무게를 실은 영리한 $\mathcal{A}$를 쓰면 $\theta$가 커져 반복 횟수가 더 줄어든다. 둘째, 성공 확률 $p$ 자체를 측정 통계 없이 추정하는 진폭 추정은 표본 수 $N$에 대해 고전 몬테카를로의 $1/\sqrt{N}$ 오차를 $1/N$로 개선한다. 이 가속은 금융 위험 평가와 수치 적분처럼 기댓값 계산이 병목인 문제에서 양자 이점의 후보로 거론된다.

다만 가속의 폭은 제곱근에 머문다. 진폭 증폭은 성공 확률을 빠르게 키울 뿐 문제 구조를 본질적으로 활용하지 않으므로, [[Shor's Algorithm|쇼어 알고리즘]]이 보이는 지수 가속과는 성격이 다르다. 또한 반복마다 $\mathcal{A}$와 그 역연산 $\mathcal{A}^{-1}$을 호출해야 하므로 회로 깊이가 반복 횟수에 비례해 깊어진다. 의미 있는 규모의 증폭을 실현하려면 깊은 회로를 끝까지 오류 없이 돌릴 수 있는 결함 허용 양자컴퓨터가 필요하다는 제약은 그로버와 동일하다.

## 연결
- [[Grover's Algorithm]] 균등 중첩과 단일 표시 해라는 특수 조건에서의 진폭 증폭이며, 진폭 증폭은 이를 임의의 출발 상태와 술어로 일반화한 기법
- [[Grover Diffusion Operator]] 출발 상태가 균등 중첩일 때의 $S_0'$ 반사에 해당하며, 진폭 증폭에서는 이를 임의의 $\mathcal{A}$ 기준 반사로 대체함
- [[Quantum Measurement]] 증폭이 키우는 것은 측정 성공 확률이며, 측정 시점을 최적 반복 횟수에 맞춰야 함을 규정하는 토대
- [[Quantum Superposition]] 성공과 실패 부분공간의 진폭이 간섭으로 재분배되는 것이 증폭의 물리적 메커니즘
- [[Shor's Algorithm]] 지수 가속과 대비되는 제곱근 가속의 대표로, 가속 폭의 한계를 보여 주는 참조점
