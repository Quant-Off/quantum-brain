---
title: SPHINCS+ (SLH-DSA)
aliases: [SLH-DSA, SPHINCS+, 스핑크스, 해시 기반 무상태 서명, Stateless Hash-Based Signature, FIPS 205]
type: concept
status: budding
domain: post-quantum-cryptography
tags:
  - pqc/signature
  - pqc/hash-based
  - nist/fips-205
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Post-Quantum Cryptography]]"
related:
  - "[[PQC 전이 감시]]"
  - "[[Dilithium (ML-DSA)]]"
  - "[[FN-DSA (Falcon)]]"
  - "[[FIPS 205]]"
  - "[[Grover's Algorithm]]"
  - "[[Crypto-Agility]]"
source:
  - "NIST FIPS 205"
confidence: high
---

# SPHINCS+ (SLH-DSA)

> 해시 함수의 안전성만을 보안 근거로 삼는 무상태(stateless) 디지털 서명 메커니즘으로, NIST가 FIPS 205(SLH-DSA)로 표준화했다.

## 핵심
SPHINCS+는 격자나 코드 같은 구조화된 수학 가정에 의존하지 않고, 오직 기반 해시 함수의 (다중 타겟) 제2 원상 저항성과 원상 저항성에서 안전성을 끌어내는 서명 방식이다. 설계 단계에서 충돌 저항성 의존을 일부러 피해, 충돌 공격이 해시를 위협하더라도 견디도록 보수성을 높인 점이 특징이다. 표준명은 SLH-DSA(Stateless Hash-Based Digital Signature Algorithm)이며, 내부적으로 SHA-2 또는 SHAKE를 인스턴스로 사용한다.

핵심 구성은 세 겹으로 쌓인다. 가장 아래에는 일회용 서명(OTS)인 [[WOTS+]]가 있고, 그 위에 다수의 일회용 키를 머클 트리로 묶는 [[XMSS]] 계열의 트리가 있으며, 이 트리들을 다시 [[Hypertree]]로 계층화한다. 일회용 서명을 메시지마다 안전하게 고르기 위해 무상태 방식인 [[FORS]](Forest of Random Subsets)를 메시지 다이제스트에 적용한다. 이 무상태 선택 덕분에 서명자가 사용한 키의 색인을 따로 저장하고 갱신할 필요가 없다. 상태를 들고 다니는 [[XMSS]]나 LMS와 갈라지는 결정적 차이가 여기에 있다.

전체 키와 서명의 무결성은 다음과 같은 검증 관계로 환원된다. 검증자는 서명에 담긴 인증 경로를 따라 머클 루트를 재계산하고, 그 값이 공개키에 박힌 루트와 일치하는지를 확인한다.

$$ \mathrm{Verify}(pk, m, \sigma) = 1 \iff \mathsf{Root}_{\text{recomputed}}(m, \sigma) = pk_{\text{root}} $$

해시 기반 서명의 보안 강도는 사용 해시의 출력 길이 $n$에 직접 묶인다. 양자 공격자가 [[Grover's Algorithm|그로버 알고리즘]]으로 일방향 함수의 원상을 탐색해도 비용은 $O(2^{n/2})$ 수준에 그치므로, 매개변수는 이 제곱근 가속까지 흡수하도록 설계한다. SLH-DSA는 보안 범주 1, 3, 5에 대응하는 매개변수 집합(예: SLH-DSA-SHA2-128s, SLH-DSA-SHAKE-256f 등)을 제공하며, 같은 보안 수준 안에서도 서명 크기가 작은 `s`(small) 변형과 서명 생성이 빠른 `f`(fast) 변형으로 나뉜다.

대가는 분명하다. 서명 크기가 수 킬로바이트에서 수십 킬로바이트에 이르고 서명 연산이 느리다. 그래서 SPHINCS+는 대량 트랜잭션용이 아니라, 보안 가정을 최대한 보수적으로 두어야 하는 펌웨어 서명이나 루트 인증서 같은 곳에 어울린다.

## 구조
```mermaid
flowchart TD
    M["메시지 다이제스트"] --> FORS["$$\text{FORS}$$ (무상태 few-time 서명)"]
    FORS --> HT["$$\text{Hypertree}$$: 다층 $$\text{XMSS}$$ 트리, 각 트리는 $$\text{WOTS+}$$ 일회용 서명으로 구성"]
    HT --> Root["$$pk_{\text{root}}$$ 최상층 XMSS 루트 = 공개키"]
```

## 왜 중요한가
[[Shor's Algorithm|쇼어 알고리즘]]이 RSA와 타원곡선 서명을 다항 시간에 무너뜨리므로 서명 알고리즘도 양자 내성으로 갈아타야 한다. NIST가 표준화한 세 서명 가운데 [[Dilithium (ML-DSA)]]와 [[FN-DSA (Falcon)]]은 격자 가정에 기대지만, SPHINCS+는 해시 안전성이라는 가장 잘 연구되고 보수적인 가정 하나에만 의존한다. 따라서 격자 계열의 안전성 가정이 미래에 흔들리더라도 무너지지 않는 수학적 백업 역할을 한다. 격자 기반 KEM에 대해 [[HQC]]가 코드 기반 백업을 맡는 구도와 같은 다양화 전략이다.

이 보수성은 [[Crypto-Agility|암호 민첩성]] 관점에서 가치가 크다. 단일 가정에 모든 서명을 거는 대신, 서로 독립적인 난해성 가정에 기반한 알고리즘을 함께 갖추면 한 가정의 붕괴가 전체 신뢰 체계를 무너뜨리지 못한다. 그래서 SPHINCS+는 일상 트래픽보다는 신뢰의 뿌리, 즉 장기 루트 키와 펌웨어 무결성처럼 한 번 깨지면 복구 비용이 막대한 자산을 떠받치는 곳에 배치된다.

## 연결
- [[MOC - Post-Quantum Cryptography]] 이 개념이 속한 도메인 지도이자 상위 진입점
- [[PQC 전이 감시]] FIPS 205 SLH-DSA를 추적 대상으로 두는 지속 관리 영역
- [[Dilithium (ML-DSA)]] 같은 NIST 서명 표준이지만 격자 가정에 기반한 1순위 서명(대비)
- [[FN-DSA (Falcon)]] 격자 기반 컴팩트 서명으로, 크기와 가정에서 SPHINCS+와 트레이드오프가 갈리는 형제 표준
- [[FIPS 205]] SLH-DSA를 규정하는 표준 문헌(출처 노트 작성 지점)
- [[Grover's Algorithm]] 해시 기반 서명의 보안 강도 매개변수를 $O(2^{n/2})$로 결정짓는 양자 공격
- [[Crypto-Agility]] 독립 가정의 백업 서명을 갖춰 단일 가정 붕괴에 대비하는 설계 원칙
