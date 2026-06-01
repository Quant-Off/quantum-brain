---
title: Hamiltonian
aliases: [해밀토니안, 해밀턴 연산자, 에너지 연산자, Hamiltonian Operator]
type: concept
status: budding
domain: foundations
tags:
  - concept/dynamics
  - concept/operator
  - concept/energy
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
related:
  - "[[Unitary Evolution]]"
  - "[[Observable (Hermitian Operator)]]"
  - "[[Pauli Matrices]]"
  - "[[Quantum Measurement]]"
confidence: high
---

# Hamiltonian

> 양자계의 총에너지에 대응하는 에르미트 연산자 $H = H^{\dagger}$ 로, 슈뢰딩거 방정식을 통해 시간 발전을 생성하며 그 고윳값이 계가 가질 수 있는 에너지 준위가 된다.

## 핵심
해밀토니안은 고전역학의 해밀턴 함수가 양자역학으로 옮겨 오면서 연산자가 된 대상이다. 고전적으로 계의 총에너지를 운동에너지와 위치에너지의 합으로 적던 양이, 양자역학에서는 [[Hilbert Space|힐베르트 공간]] 위에 작용하는 선형연산자 $H$ 로 승격된다. 이 연산자는 에너지라는 실제로 측정 가능한 물리량을 나타내므로 [[Observable (Hermitian Operator)|관측가능량]]의 일종이며, 따라서 켤레전치가 자기 자신과 같은 에르미트 조건을 만족한다.

$$ H = H^{\dagger} $$

에르미트성 덕분에 $H$ 의 고윳값은 모두 실수이고, 그 고유상태들은 정규직교 기저를 이룬다. 고유값 방정식은 다음과 같이 적힌다.

$$ H\,\lvert E_n \rangle = E_n\,\lvert E_n \rangle $$

여기서 실수 $E_n$ 은 계가 측정에서 가질 수 있는 에너지 값이고, 켓 $\lvert E_n \rangle$ 은 그에 대응하는 에너지 고유상태이다. 가장 낮은 고윳값을 주는 상태를 바닥상태라 부르며, 그 위의 준위들이 들뜬상태가 된다. 이산적인 에너지 준위라는 양자역학의 상징적 특징은 바로 이 고윳값 스펙트럼에서 나온다.

해밀토니안의 가장 중요한 역할은 계의 동역학을 생성하는 데 있다. 측정이 개입하지 않는 닫힌 계의 상태는 시간에 따라 [[Schrödinger Equation|슈뢰딩거 방정식]]을 따라 변한다.

$$ i\hbar\,\frac{d}{dt}\lvert\psi(t)\rangle = H\,\lvert\psi(t)\rangle $$

이 방정식에서 $H$ 가 시간에 무관하면 형식적인 해를 지수 사상으로 적을 수 있고, 그 결과가 곧 [[Unitary Evolution|유니터리 발전]]의 발전 연산자이다.

$$ U(t) = \exp\!\left(-\frac{i H t}{\hbar}\right) $$

$H$ 의 에르미트성은 이 $U(t)$ 의 유니터리성을 직접 보장한다. $U^{\dagger}(t) = \exp(+iHt/\hbar)$ 이므로 $U^{\dagger}U = I$ 가 곧바로 따라 나오기 때문이다. 이런 의미에서 해밀토니안은 시간 발전의 무한소 생성자(generator)라 불린다. 한 순간의 무한소 변화율을 정하는 연산자가 $H$ 이고, 그것을 시간 방향으로 누적해 지수화한 것이 유한 시간의 발전 $U(t)$ 인 셈이다.

에너지 고유상태가 시간에 따라 어떻게 변하는지를 보면 이 구조가 더 분명해진다. $H\lvert E_n\rangle = E_n\lvert E_n\rangle$ 인 상태는 발전 연산자를 적용해도 전체 위상만 회전할 뿐 물리적 상태가 바뀌지 않는다.

$$ \lvert\psi(t)\rangle = e^{-iE_n t/\hbar}\,\lvert E_n \rangle $$

그래서 에너지 고유상태를 정상상태(stationary state)라 부른다. 임의의 초기 상태는 이 에너지 고유상태들의 중첩으로 전개되며, 각 성분이 서로 다른 속도로 위상을 돌면서 전체 상태의 시간 변화가 만들어진다.

## 구조
```mermaid
flowchart LR
    H["$$H = H^{\dagger}$$ (총에너지)"] -->|고윳값 방정식| SPEC["$$H\lvert E_n\rangle = E_n\lvert E_n\rangle$$ 에너지 준위"]
    H -->|슈뢰딩거 방정식의 생성자| SE["$$i\hbar\,\tfrac{d}{dt}\lvert\psi\rangle = H\lvert\psi\rangle$$"]
    SE -->|시간 무관 시 지수화| U["$$U(t)=\exp(-iHt/\hbar)$$"]
    U -->|$$H=H^{\dagger}$$ 보장| UNI["유니터리 발전 (가역)"]
```

## 왜 중요한가
해밀토니안은 양자계가 무엇을 할 수 있는지를 한 연산자에 압축해 담는다. 계의 정적 정보인 가능한 에너지 준위와, 동적 정보인 시간 발전이 모두 $H$ 하나에서 결정되기 때문이다. 양자정보의 관점에서 이는 두 가지 의미를 가진다.

첫째, 양자 게이트와 양자 알고리즘의 물리적 실행은 결국 해밀토니안을 설계하고 제어하는 일이다. 실험가가 큐비트에 가하는 펄스나 자기장은 일정 시간 동안 특정 $H$ 를 켜는 행위이며, 그 결과 얻어지는 $U(t) = \exp(-iHt/\hbar)$ 가 원하는 [[Pauli Matrices|단일 큐비트 게이트]]나 다중 큐비트 게이트가 되도록 맞춘다. 이 사고를 가장 직접적으로 사용하는 패러다임이 [[Adiabatic Quantum Computation|단열 양자 계산]]과 [[Quantum Annealing|양자 어닐링]]으로, 풀고자 하는 문제의 답을 어떤 해밀토니안의 바닥상태에 부호화한 뒤 계를 그 바닥상태로 천천히 이끈다.

둘째, 해밀토니안 자체를 계산의 타겟으로 삼는 문제군이 있다. 분자나 물질의 바닥상태 에너지를 구하는 양자 화학 시뮬레이션, 그리고 [[Hamiltonian Simulation|해밀토니안 시뮬레이션]]이 대표적이다. 고전 컴퓨터로는 차원이 지수적으로 커지는 다체계 해밀토니안의 발전을, 양자 컴퓨터는 자연스럽게 흉내 낼 수 있다는 점이 양자 우월성 논의의 한 축을 이룬다. 결국 해밀토니안은 에너지의 표상이면서 동시에 시간이라는 자원을 계산으로 바꾸는 다리이다.

## 연결
- [[Unitary Evolution]] 해밀토니안을 지수화해 얻는 발전 연산자가 닫힌 계의 시간 발전을 기술한다
- [[Observable (Hermitian Operator)]] 해밀토니안은 에너지라는 측정 가능량을 나타내는 에르미트 연산자의 한 사례
- [[Pauli Matrices]] 큐비트 해밀토니안을 구성하고 게이트로 지수화하는 기본 연산자 집합
- [[Quantum Measurement]] 해밀토니안의 고윳값이 에너지 측정 결과로 나타나는 관측 과정
- [[Schrödinger Equation]] 해밀토니안이 생성자로 들어가는 시간 발전의 지배 방정식
