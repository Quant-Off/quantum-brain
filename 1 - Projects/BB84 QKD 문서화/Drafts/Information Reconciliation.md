---
title: Information Reconciliation
aliases: [정보 조정, 정보 화해, information reconciliation, 오류 정정]
type: concept
status: seedling
domain: quantum-cryptography
tags:
  - qkd/bb84
  - qkd/key-distillation
  - qkd/error-correction
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Quantum Cryptography]]"
related:
  - "[[BB84 Protocol]]"
  - "[[Quantum Bit Error Rate (QBER)]]"
  - "[[Basis Sifting]]"
  - "[[Privacy Amplification]]"
  - "[[LDPC Codes]]"
  - "[[Cascade Protocol]]"
source:
  - "G. Brassard and L. Salvail, Secret-Key Reconciliation by Public Discussion, EUROCRYPT 1993"
confidence: high
---
# Information Reconciliation

> 정보 조정은 시프트 키에 남은 Alice와 Bob 사이의 불일치 비트를 인증된 공개 채널 통신으로 정정해 두 사람이 동일한 비트열을 갖게 하는 키 증류 단계다.

## 핵심
[[Basis Sifting|기저 시프팅]]을 마치면 Alice와 Bob은 기저가 일치한 위치의 비트만 남긴 시프트 키를 갖는다. 그러나 채널 잡음, 장비 불완전성, 그리고 Eve의 도청 때문에 두 사람의 시프트 키는 완전히 같지 않고 일부 위치에서 비트가 어긋난다. 이 불일치 비율이 [[Quantum Bit Error Rate (QBER)|QBER]]이며, 정보 조정의 입력 오류율을 결정한다. 정보 조정은 인증된 공개 채널에서 패리티 정보를 주고받아 이 어긋난 비트들을 찾아 정정하고, 두 사람의 키를 동일하게 맞추는 과정이다.

대표 기법은 대화형 [[Cascade Protocol|Cascade]]와 [[LDPC Codes|LDPC]] 기반 일방향 부호화, 그리고 Winnow 등이다. Cascade는 블록을 나누어 패리티를 비교하고 불일치 블록을 이진 탐색으로 좁혀 오류를 잡은 뒤, 여러 패스에 걸쳐 블록 경계를 뒤섞으며 반복한다. 구현이 단순하고 효율이 좋지만 왕복 통신이 많다. LDPC 부호는 신드롬을 한 번에 보내 왕복을 줄이는 일방향 방식으로, 고속 QKD 시스템에 유리하다.

정보 조정의 본질적 비용은 패리티 누설이다. 공개 채널로 주고받는 모든 패리티와 신드롬은 인증되어 있어 변조는 막지만 비밀은 아니므로 Eve에게도 그대로 노출된다. Eve가 이 누설로 얻은 정보량은 이후 [[Privacy Amplification|비밀성 증폭]]에서 반드시 차감해야 하며, 따라서 정정 자체보다 누설 최소화가 관건이다. 이 점에서 정보 조정은 고전 통신의 순수한 오류정정과 성격이 다르다. 고전 부호는 채널을 통과한 비트를 복원하는 데 집중하지만, 정보 조정은 오류를 잡으면서 동시에 공개로 흘러나가는 정보량을 줄이는 이중 목표를 갖는다.

조정 효율은 보통 계수 $f \ge 1$로 표현한다. 길이 $n$의 시프트 키에서 오류율이 $Q$일 때 이상적 한계는 Shannon 한계인 $n\,h(Q)$ 비트의 누설이며, 여기서 $h(\cdot)$는 이진 엔트로피

$$ h(Q) = -Q\log_2 Q - (1-Q)\log_2(1-Q) $$

이다. 실제 기법이 공개하는 정보량 $\mathrm{leak}_{\mathrm{EC}}$는 이 한계보다 크므로

$$ \mathrm{leak}_{\mathrm{EC}} = f \cdot n\, h(Q), \qquad f \ge 1 $$

로 쓴다. $f = 1$이 이상적 한계이고, $f$가 1에 가까울수록 효율이 좋다. 잘 조정된 Cascade와 LDPC는 실용 영역에서 대략 $f \approx 1.05$에서 $1.2$ 수준을 달성한다. 누설량이 클수록 비밀성 증폭에서 더 많은 비트를 깎아내야 하므로 최종 비밀 키 길이가 줄어든다.

## 흐름
```mermaid
flowchart LR
    S["시프트 키 (Alice, Bob)"] --> Q["$$Q$$ = QBER 입력"]
    Q --> R["정보 조정: 패리티 교환과 오류 정정"]
    R --> M["병합된 동일 비트열"]
    R -. "공개 누설 $$f\,n\,h(Q)$$" .-> P["Privacy Amplification에서 차감"]
    M --> P
    P --> K["최종 비밀 키"]
```

## 왜 중요한가
정보 조정이 없으면 Alice와 Bob은 서로 미묘하게 다른 키를 갖게 되고, 그 상태로는 동일 키를 전제로 하는 이후 단계가 모두 무너진다. 비밀성 증폭은 두 사람이 같은 비트열을 갖고 있다는 가정 위에서 해시를 적용하므로, 키가 어긋나 있으면 출력이 달라져 쓸 수 없다. 따라서 정보 조정은 증류 파이프라인에서 동일 키를 보장하는 필수 관문이며, [[Basis Sifting]]과 [[Privacy Amplification]] 사이를 잇는 단계다. 동시에 이 단계의 누설량이 최종 키율을 직접 좌우하므로, QKD 시스템의 처리량과 보안 마진을 함께 결정하는 핵심 설계 지점이기도 하다.

## 연결
- [[BB84 Protocol]] 정보 조정이 키 증류 단계로 편입되는 대표 프로토콜
- [[Quantum Bit Error Rate (QBER)]] 정보 조정의 입력 오류율 $Q$를 결정하는 지표
- [[Basis Sifting]] 정보 조정의 입력인 시프트 키를 만들어 내는 직전 단계
- [[Privacy Amplification]] 정보 조정의 공개 누설량을 차감해 비밀 키를 정제하는 다음 단계
- [[LDPC Codes]] 신드롬을 한 번에 보내 왕복을 줄이는 일방향 정보 조정 부호로 고속 QKD에 쓰임
- [[Cascade Protocol]] 정보 조정을 구현하는 대표 대화형 기법으로 블록 패리티와 이진 탐색으로 오류를 잡음
