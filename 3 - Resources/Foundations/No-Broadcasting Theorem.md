---
title: No-Broadcasting Theorem
aliases: [방송 불가 정리, 브로드캐스팅 불가 정리, 복제 불가의 혼합 상태 확장, No-Broadcasting]
type: concept
status: budding
domain: foundations
tags:
  - concept/no-cloning
  - concept/theorem
  - concept/mixed-state
  - concept/density-matrix
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
related:
  - "[[No-Cloning Theorem]]"
  - "[[Density Matrix]]"
  - "[[Partial Trace]]"
  - "[[Quantum Entanglement]]"
  - "[[Quantum Measurement]]"
source:
  - "Barnum, Caves, Fuchs, Jozsa, Schumacher, Noncommuting Mixed States Cannot Be Broadcast, Phys. Rev. Lett. 76, 2818 (1996)"
confidence: high
---

# No-Broadcasting Theorem

> 비가환인 두 혼합 상태로 이루어진 집합은 단일 양자 연산으로 방송(broadcast)할 수 없다는 정리로, 혼합 상태 영역으로 확장한 복제 불가성의 일반화다.

## 핵심
[[No-Cloning Theorem|복제 불가 정리]]는 임의의 미지 순수 상태 $\lvert\psi\rangle$ 를 그대로 한 벌 더 만드는 보편적 [[Unitary Evolution|유니터리]]가 없음을 말한다. 그런데 정보의 일반적 기술 대상은 순수 상태가 아니라 [[Density Matrix|밀도 행렬]] $\rho$ 로 적히는 혼합 상태다. 또한 복제보다 약한 요구가 있다. 사본 두 벌이 통째로 같을 필요 없이, 두 출력 시스템 각각을 따로 떼어 보았을 때(즉 [[Partial Trace|부분 대각합]]을 취했을 때) 그것이 원래 상태와 같기만 하면 충분하다고 풀어 줄 수 있다. 이렇게 약화한 복사를 방송(broadcasting)이라 부른다.

방송은 입력 상태 $\rho$ 와 표준 초기 상태 $\sigma_0$ 에 놓인 빈 시스템을 받아, 어떤 양자 연산 $\mathcal{E}$ 를 거쳐 두 시스템 $A$ 와 $B$ 의 결합 상태 $\Lambda$ 를 내놓는 과정이다. 다음 두 조건을 동시에 만족하면 $\rho$ 가 방송된 것이다.

$$ \mathrm{Tr}_{B}\left[ \Lambda \right] = \rho, \qquad \mathrm{Tr}_{A}\left[ \Lambda \right] = \rho $$

여기서 결합 상태 $\Lambda$ 자체는 $\rho \otimes \rho$ 일 필요가 없고, 두 부분계가 서로 상관(상관관계나 [[Quantum Entanglement|얽힘]])을 가져도 된다. 각 변두리만 $\rho$ 와 일치하면 된다는 점이 복제보다 약한 요구다. 복제가 가능하면 방송도 자동으로 가능하지만, 그 역은 일반적으로 성립하지 않으므로 방송 불가성은 더 강한 금지 진술이다.

핵심 결과는 다음과 같다. 두 상태 $\rho_1$ 과 $\rho_2$ 를 모두 방송하는 단일 연산 $\mathcal{E}$ 가 존재할 필요충분조건은 두 밀도 행렬이 서로 가환인 것이다.

$$ \left[ \rho_1, \rho_2 \right] = \rho_1 \rho_2 - \rho_2 \rho_1 = 0 $$

가환이면 두 상태는 공통의 고유기저에서 동시에 대각화되어, 사실상 그 기저의 고전 확률분포처럼 다룰 수 있다. 고전 확률분포는 자유롭게 복사되므로 방송이 가능하다. 반대로 두 상태가 비가환이면, 공통 고유기저가 없어 단일 연산으로 양쪽 변두리를 동시에 원래대로 맞출 수 없고 방송은 불가능하다. 직교하지 않는 순수 상태 한 쌍은 항상 비가환이므로, 순수 상태에 대한 복제 불가 정리가 이 결과의 특수한 경우로 흡수된다.

## 구조
```mermaid
flowchart TD
    A["입력 $$\rho$$ 와 빈 시스템 $$\sigma_0$$"] --> B["양자 연산 $$\mathcal{E}$$ 적용"]
    B --> C["결합 출력 $$\Lambda$$ (상관 허용)"]
    C --> D["방송 조건: $$\mathrm{Tr}_B[\Lambda]=\rho$$ 와 $$\mathrm{Tr}_A[\Lambda]=\rho$$"]
    D --> E{"두 상태가 가환? $$[\rho_1,\rho_2]=0$$"}
    E -- "가환" --> F["공통 고유기저에서 고전 분포처럼 방송 가능"]
    E -- "비가환" --> G["단일 연산으로 양 변두리 동시 복원 불가, 방송 불가"]
```

## 왜 중요한가
방송 불가 정리는 복제 불가 정리가 순수 상태에 한정된 진술이라는 한계를 넘어, 혼합 상태와 약화한 복사 요구까지 포괄하는 가장 일반적인 금지 정리다. 이로써 양자정보를 자유롭게 퍼뜨릴 수 없다는 사실이 특수한 순수 상태의 우연이 아니라 비가환성이라는 양자 구조의 본질적 귀결임이 드러난다.

응용 측면에서 이 정리는 양자정보의 분배와 복사에 근본적 제약을 건다. 한 송신자가 보유한 미지의 혼합 상태를 여러 수신자에게 그대로 뿌리는 일이 비가환 집합에 대해서는 원천적으로 막히므로, [[Quantum Cryptography|양자암호]]에서 도청자가 상태를 손실 없이 가로채 분배하는 시나리오 역시 차단된다. 또한 양자정보를 여러 부분계에 흩뿌리려 할 때 변두리 충실도(fidelity)와 부분계 사이 상관 사이에 상충이 생긴다는 점은, 정보의 국소화와 공유에 관한 정량적 한계로 이어진다.

여기서 두 가지 오해를 정리해 둘 필요가 있다. 첫째, 방송은 근사적으로는 일부 가능하다. 정리가 금지하는 것은 완벽한 방송이며, 충실도를 1보다 낮추면 보편적 양자 복제기(universal quantum cloning machine) 같은 근사 방송이 존재한다. 둘째, 단일 고정 상태 하나만 다룬다면 그것을 미리 안다는 뜻이므로 얼마든지 다시 준비해 뿌릴 수 있다. 금지가 발동하는 핵심은 어디까지나 서로 비가환인 상태들로 이루어진 집합을 단일 연산으로 동시에 처리하려 할 때다.

## 연결
- [[No-Cloning Theorem]] 순수 상태에 한정된 복제 불가성을 혼합 상태와 약화한 방송 요구로 일반화한 상위 정리
- [[Density Matrix]] 방송의 입출력 상태를 기술하는 혼합 상태 형식이자 가환성 판정의 무대
- [[Partial Trace]] 결합 출력에서 각 변두리를 떼어 내 방송 조건을 정의하는 연산
- [[Quantum Entanglement]] 방송 출력의 두 부분계가 가질 수 있는 상관의 한 형태이자 변두리 복원과 상충하는 자원
- [[Quantum Measurement]] 미지 상태를 측정으로 알아내 재준비하는 우회 방송마저 막는 측정의 정보 한계
