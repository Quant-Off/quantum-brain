---
title: Unitary Evolution
aliases: [유니터리 시간발전, 유니터리 연산, 가역 발전, Unitary Operator]
type: concept
status: budding
domain: foundations
tags:
  - concept/dynamics
  - concept/operator
  - concept/reversibility
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
related:
  - "[[Schrödinger Equation]]"
  - "[[Hamiltonian]]"
  - "[[Pauli Matrices]]"
  - "[[Quantum Measurement]]"
  - "[[Hilbert Space]]"
  - "[[No-Cloning Theorem]]"
confidence: high
---

# Unitary Evolution

> 외부와 단절된 고립계의 시간 발전은 내적과 노름을 보존하는 유니터리 연산자 $U$ 로 기술된다는 양자역학의 공준이며, $U^{\dagger}U = U U^{\dagger} = I$ 를 만족하므로 항상 가역적이다.

## 핵심
양자역학의 동역학 공준은 측정이 개입하지 않는 닫힌 계(고립계)의 상태가 어떤 켓에서 다른 켓으로 어떻게 옮겨 가는지를 규정한다. 시각 $t_1$ 의 상태 $\lvert\psi(t_1)\rangle$ 와 시각 $t_2$ 의 상태는 [[Hilbert Space]] 위의 한 선형연산자 $U$ 로 이어진다.

$$ \lvert\psi(t_2)\rangle = U(t_2, t_1)\,\lvert\psi(t_1)\rangle $$

여기서 $U$ 는 임의의 선형연산자가 아니라 반드시 유니터리(unitary)여야 한다. 즉 그 켤레전치가 곧 역연산자가 된다.

$$ U^{\dagger}U = U U^{\dagger} = I $$

이 조건이 물리적으로 요구되는 이유는 확률 보존에 있다. 두 상태의 내적은 유니터리 변환 아래에서 그대로 유지된다. 임의의 두 켓 $\lvert\phi\rangle$, $\lvert\psi\rangle$ 에 대해 다음이 성립한다.

$$ \langle U\phi \vert U\psi \rangle = \langle\phi\rvert U^{\dagger}U \lvert\psi\rangle = \langle\phi\vert\psi\rangle $$

특히 $\lvert\phi\rangle = \lvert\psi\rangle$ 로 두면 노름이 보존되므로 $\langle\psi\vert\psi\rangle = 1$ 이라는 정규화가 시간이 지나도 깨지지 않는다. 노름의 제곱이 측정 확률의 총합에 해당하므로, 유니터리성은 곧 전체 확률이 항상 $1$ 로 유지된다는 진술이다. 그리고 $U^{\dagger}$ 라는 역연산자가 언제나 존재하므로 발전은 가역적이다. 발전 후의 상태에 $U^{\dagger}$ 를 작용시키면 원래 상태가 정확히 복원되어 정보가 소실되지 않는다.

연속 시간 발전의 구체적 형태는 [[Schrödinger Equation|슈뢰딩거 방정식]]에서 나온다. 계의 총에너지를 나타내는 에르미트 연산자 [[Hamiltonian|해밀토니안]] $H$ 가 주어지면 상태는 다음을 따른다.

$$ i\hbar\,\frac{d}{dt}\lvert\psi(t)\rangle = H\,\lvert\psi(t)\rangle $$

$H$ 가 시간에 무관할 때 이 방정식의 해는 지수 형태의 발전 연산자로 적힌다.

$$ U(t) = \exp\!\left(-\frac{i H t}{\hbar}\right) $$

$H$ 가 에르미트($H = H^{\dagger}$)라는 사실이 $U$ 의 유니터리성을 보장한다. $U^{\dagger} = \exp(+iHt/\hbar)$ 이므로 $U^{\dagger}U = I$ 가 곧바로 따라 나온다. 즉 관측가능량의 실수성을 주는 에르미트성과 시간 발전의 확률 보존성을 주는 유니터리성은 지수 사상으로 맞물려 있다.

이 추상적 발전이 양자컴퓨팅에서 구체화된 것이 양자 게이트다. 모든 양자 게이트는 큐비트 상태에 작용하는 유니터리 연산자이며, 대표적인 예가 단일 큐비트 연산의 기본 집합인 [[Pauli Matrices|파울리 행렬]]이다. 예를 들어 $X$ 게이트는 $X^{\dagger}X = I$ 를 만족하고 자기 자신이 역연산이라 $X^2 = I$ 이다. 회로 전체의 동작 역시 개별 게이트 유니터리들의 곱이므로 그 자체로 하나의 유니터리 발전이다.

반면 [[Quantum Measurement|측정]]은 이 그림에서 벗어난다. 측정은 유니터리가 아니라 사영(projection)과 정규화로 기술되는 비선형 과정이며, 측정 결과가 무엇이 될지는 확률적으로만 정해지고 측정 후 상태로부터 측정 전 상태를 되돌릴 수 없다. 즉 닫힌 계의 결정적이고 가역적인 유니터리 발전과, 외부 관측이 개입하는 비유니터리이고 비가역적인 측정 붕괴는 양자역학의 두 갈래 동역학을 이룬다.

## 구조
```mermaid
flowchart LR
    H["해밀토니안 H (에르미트)"] -->|exp(-iHt/hbar)| U["발전 연산자 U"]
    U -->|U^dagger U = I| PROP["내적·노름 보존"]
    PROP --> REV["가역성: U^dagger 로 복원"]
    U -->|특수 사례| GATE["양자 게이트 (파울리 등)"]
    U -.대조.-> M["측정: 비유니터리·비가역"]
```

## 왜 중요한가
유니터리 발전은 양자정보가 고전정보와 갈라서는 지점을 형식적으로 못 박는다. 첫째, 가역성은 양자 계산이 원리적으로 정보를 버리지 않음을 뜻한다. 모든 게이트가 역을 가지므로 회로는 거꾸로 돌릴 수 있고, 이는 비가역 게이트를 자유롭게 쓰는 고전 논리와 근본적으로 다르다.

둘째, 선형성과 유니터리성은 [[No-Cloning Theorem|복제 불가 정리]]의 직접적 근거가 된다. 임의의 미지 상태 $\lvert\psi\rangle$ 를 빈 상태 $\lvert e\rangle$ 위에 복제하는 유니터리 $U$ 가 있다고 가정하면 $U\lvert\psi\rangle\lvert e\rangle = \lvert\psi\rangle\lvert\psi\rangle$ 여야 한다. 그러나 서로 다른 두 입력 $\lvert\psi\rangle$, $\lvert\phi\rangle$ 에 같은 $U$ 를 적용한 뒤 내적을 비교하면 $\langle\phi\vert\psi\rangle = (\langle\phi\vert\psi\rangle)^2$ 라는 모순이 생긴다. 내적을 보존해야 하는 유니터리성이 곧 임의 상태의 복제를 가로막는 셈이다. 결국 양자 키 분배 같은 보안 응용의 토대도 이 동역학 공준에서 출발한다.

## 연결
- [[Hamiltonian]] 지수화하면 발전 연산자 $U(t)$ 가 되는 시간 발전의 생성자
- [[Pauli Matrices]] 단일 큐비트에 작용하는 가장 기본적인 유니터리 게이트의 예
- [[Quantum Measurement]] 유니터리 발전과 대비되는 비유니터리이고 비가역인 동역학
- [[Hilbert Space]] 유니터리 연산자가 작용하고 내적이 정의되는 상태 공간
- [[No-Cloning Theorem]] 유니터리성과 선형성에서 따라 나오는 복제 불가 한계
