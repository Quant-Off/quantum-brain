---
title: Measurement-Based Quantum Computing
aliases: [MBQC, 측정 기반 양자 계산, 일방향 양자 계산, One-Way Quantum Computer, 클러스터 상태 계산]
type: concept
status: budding
domain: quantum-computing
tags:
  - concept/measurement
  - concept/entanglement
  - concept/cluster-state
  - hardware/photonic
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Quantum Computing]]"
related:
  - "[[Cluster State]]"
  - "[[Quantum Measurement]]"
  - "[[Quantum Entanglement]]"
  - "[[Photonic Qubit]]"
confidence: high
---

# Measurement-Based Quantum Computing

> 대규모로 얽힌 자원 상태를 먼저 준비한 뒤, 단일 큐비트 측정의 순서와 기저 선택만으로 계산을 진행하는 양자 계산 모형이다.

## 핵심
회로 모형(Circuit Model)에서는 입력 상태에 유니터리 게이트를 차례로 적용하면서 계산이 진행되고, 측정은 마지막에 결과를 읽기 위해서만 한다. 측정 기반 양자 계산은 이 순서를 뒤집는다. 계산에 필요한 얽힘을 모두 담은 자원 상태를 처음에 한꺼번에 만들어 두고, 이후에는 큐비트를 하나씩 측정하면서 계산을 끌고 간다. 게이트를 능동적으로 거는 단계가 따로 없고, 측정을 통해 얽힘을 점진적으로 소비하기 때문에 일방향 양자 계산(One-Way Quantum Computer)이라고도 부른다.

자원 상태로는 보통 [[Cluster State|클러스터 상태]]를 쓴다. 격자 위에 놓인 각 큐비트를 $\lvert + \rangle = \tfrac{1}{\sqrt{2}}(\lvert 0 \rangle + \lvert 1 \rangle)$로 초기화한 뒤, 이웃한 큐비트 쌍마다 제어 위상 게이트 $CZ$를 걸어 얽힘을 심는다. 이렇게 준비한 상태는 [[Quantum Entanglement|양자 얽힘]]이 격자 전체에 퍼져 있어, 한 큐비트의 측정 결과가 그래프 구조를 따라 이웃의 상태에 영향을 준다.

계산의 실제 진행은 [[Quantum Measurement|측정]]으로 이루어진다. 각 큐비트를 $XY$ 평면의 기저
$$ \lvert \pm_\theta \rangle = \tfrac{1}{\sqrt{2}}\left( \lvert 0 \rangle \pm e^{i\theta} \lvert 1 \rangle \right) $$
로 측정하며, 측정각 $\theta$를 어떻게 고르느냐에 따라 인접 큐비트에 어떤 단일 큐비트 유니터리가 가해지는지가 결정된다. $Z$ 기저 측정은 해당 큐비트를 그래프에서 떼어내 클러스터의 모양 자체를 깎아낸다. 이렇게 측정 패턴(measurement pattern)을 설계하면 클러스터의 한 방향을 따라 논리 정보가 사실상 텔레포테이션되며 흐르고, 그 과정에서 원하는 게이트가 적용된다.

측정은 본질적으로 확률적이라 결과가 $+1$인지 $-1$인지 미리 정할 수 없다. 그래서 MBQC는 이전 측정 결과에 따라 다음 측정의 기저를 보정하는 적응적 순서를 둔다. 부수적으로 끼어든 파울리 오류는 뒤따르는 측정각과 최종 출력의 부호를 고전적으로 되짚어 상쇄한다. 이 적응성과 보정이 결정론적 계산을 가능하게 하는 핵심이며, 측정 순서에는 한 방향의 시간적 흐름이 생긴다.

## 흐름
```mermaid
flowchart LR
    A["자원 상태 준비<br/>모든 큐비트 $$\lvert + \rangle$$"] --> B["이웃 쌍에 $$CZ$$ 적용<br/>클러스터 상태 생성"]
    B --> C["적응적 단일 큐비트 측정<br/>측정각 $$\theta_j$$ 선택"]
    C --> D["이전 결과로 다음 기저 보정"]
    D --> C
    C --> E["남은 큐비트가 논리 출력"]
```

## 왜 중요한가
MBQC는 회로 모형과 계산 능력이 동등하다. 즉 회로 모형으로 할 수 있는 모든 양자 계산을 클러스터 상태와 측정만으로 똑같이 해낼 수 있다. 그런데도 별개의 모형으로서 가치가 큰 이유는 얽힘 생성과 계산 진행이 시간상 깔끔하게 분리되기 때문이다. 어렵고 오류가 잦은 다중 큐비트 얽힘 연산을 자원 준비 단계로 몰아넣고, 계산 단계는 비교적 다루기 쉬운 단일 큐비트 측정만으로 채울 수 있다.

이 구조는 [[Photonic Qubit|광자 큐비트]] 플랫폼과 특히 잘 맞는다. 광자는 서로 상호작용시키기 어려워 두 큐비트 게이트를 결정론적으로 거는 일이 까다로운데, MBQC에서는 그런 게이트를 매 단계 반복할 필요가 없다. 대신 확률적 얽힘 연산을 미리 충분히 시도해 큰 클러스터를 짜두고, 단일 광자 측정으로 계산을 끌고 가면 된다. 측정은 광자 검출기로 빠르고 안정적으로 수행할 수 있어, 융합 기반(fusion-based) 광자 양자 컴퓨팅 같은 확장 가능한 아키텍처의 이론적 토대가 된다. 또한 위상 정렬된 클러스터 위에서 오류정정 부호를 함께 엮는 결함 허용 측정 기반 계산으로도 자연스럽게 이어진다.

## 연결
- [[Cluster State]] MBQC가 소비하는 표준 자원 상태이자 그래프 상태의 대표 사례
- [[Quantum Measurement]] 계산을 능동적으로 진행시키는 수단으로 측정각 선택이 게이트를 결정
- [[Quantum Entanglement]] 자원 상태에 미리 심어 두는 계산의 원천이며 측정으로 점진적으로 소비됨
- [[Photonic Qubit]] 결정론적 두 큐비트 게이트가 어려운 광자 플랫폼에 MBQC가 잘 들어맞는 이유
- [[Quantum Teleportation]] 측정으로 논리 정보가 클러스터를 따라 전달되는 메커니즘의 기반 원리
