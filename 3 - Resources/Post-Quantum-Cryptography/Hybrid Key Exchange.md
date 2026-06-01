---
title: Hybrid Key Exchange
aliases: [하이브리드 키 교환, 하이브리드 키 합의, Hybrid KEM, 하이브리드 KEM, PQC 하이브리드]
type: concept
status: budding
domain: post-quantum-cryptography
tags:
  - pqc/kem
  - migration/hybrid
  - threat/harvest-now-decrypt-later
  - nist/fips-203
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Post-Quantum Cryptography]]"
related:
  - "[[PQC 전이 감시]]"
  - "[[Kyber (ML-KEM)]]"
  - "[[Crypto-Agility]]"
  - "[[Harvest Now Decrypt Later]]"
source:
  - "NIST SP 800-56C Rev. 2"
  - "RFC 9370"
  - "draft-ietf-tls-hybrid-design"
confidence: high
---

# Hybrid Key Exchange

> 고전 키 교환과 양자 내성 KEM을 동시에 수행하고 두 결과를 하나의 공유 비밀로 병합하여, 어느 한쪽 가정이 무너져도 다른 쪽이 보안을 떠받치게 하는 전이기 배치 방식이다.

## 핵심

하이브리드 키 교환은 두 개 이상의 독립된 키 합의 메커니즘을 같은 핸드셰이크 안에서 함께 돌린 뒤, 그 출력들을 키 유도 함수로 묶어 단일 세션 키를 만든다. 전형적인 구성은 잘 검증된 고전 알고리즘인 [[ECDH|타원곡선 디피헬만]]과 격자 기반 [[Kyber (ML-KEM)|ML-KEM]] 같은 PQC KEM을 함께 쓰는 형태다. 어느 한쪽이 단독으로 깨져도 공유 비밀 전체가 노출되지 않는 것이 설계의 핵심이다.

두 메커니즘이 각각 비밀 $z_1$과 $z_2$를 산출하면, 최종 비밀은 두 값을 이어 붙여 키 유도 함수에 통과시켜 얻는다.

$$ K = \mathrm{KDF}\!\left( z_1 \,\Vert\, z_2 \,\Vert\, \mathrm{ctx} \right) $$

여기서 $\mathrm{ctx}$는 트랜스크립트나 라벨 같은 문맥 바인딩 정보다. 이 결합이 안전하려면 결합자가 입력 중 적어도 하나가 균등하게 무작위이고 비밀이면 출력도 안전하게 만드는 성질을 가져야 한다. 이것이 하이브리드 결합자의 보안 목표이며, 공격자 우위는 두 구성 요소 중 더 강한 쪽 이상으로 유지된다. 즉 공격자가 세션 키를 구별하려면 $z_1$과 $z_2$를 모두 깨야 하므로,

$$ \mathrm{Adv}^{\text{IND}}_{\mathcal{A}}(K) \le \min\!\left( \mathrm{Adv}_{\mathcal{A}}(z_1),\ \mathrm{Adv}_{\mathcal{A}}(z_2) \right) $$

수준으로 묶인다. 단순히 $K_1$과 $K_2$를 XOR하는 방식은 한쪽 입력이 공격자에게 알려지면 무너질 수 있으므로, 표준은 연접 이후 [[HKDF]] 같은 추출 기반 KDF를 통과시키는 방식을 권장한다.

## 흐름

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Server
    C->>S: 고전 공개값 (ECDH) + PQC 캡슐화 요청
    S->>C: 고전 공개값 + PQC ciphertext
    Note over C,S: 각자 $$z_1$$ (ECDH) 와 $$z_2$$ (ML-KEM) 도출
    Note over C,S: $$K = \mathrm{KDF}(z_1 \Vert z_2 \Vert \mathrm{ctx})$$ 로 병합
```

## 왜 중요한가

전이기의 근본 딜레마는 신뢰의 비대칭이다. RSA와 [[ECDH]] 같은 고전 알고리즘은 수십 년간 공격에 노출되며 검증됐지만 [[Shor's Algorithm|쇼어 알고리즘]]에 취약하고, [[Kyber (ML-KEM)|ML-KEM]] 같은 신규 PQC는 양자 공격에 강하다고 보지만 구현 성숙도와 부채널 내성, 매개변수 안전성에 대한 실전 검증 기간이 아직 짧다. 하이브리드는 이 둘을 병합해 어느 가정이 틀리더라도 보안이 유지되도록 위험을 분산한다.

특히 [[Harvest Now Decrypt Later|지금 수집하고 나중에 복호화]] 위협은 오늘의 암호문이 미래의 양자컴퓨터로 소급 복호화될 수 있음을 뜻하므로, [[Cryptographically Relevant Quantum Computer|CRQC]]가 등장하기 전부터 PQC 보호가 필요하다. 동시에 신규 알고리즘 단독 배치의 위험을 피하려는 보수성도 요구된다. 하이브리드는 이 두 요구를 동시에 만족시키는 현실적 절충이며, 실제로 TLS 1.3의 [[X25519MLKEM768]] 키 교환 그룹이 주요 브라우저와 서버에서 기본 활성화되는 형태로 이미 광범위하게 배치되고 있다. 이런 배치가 가능하려면 알고리즘을 손쉽게 협상하고 교체할 수 있어야 하므로, 하이브리드는 [[Crypto-Agility|암호 민첩성]]을 전제로 한다. 표준 측면에서는 NIST SP 800-56C가 하이브리드 결합 KDF의 형태를, IETF의 TLS 하이브리드 설계 문서와 RFC 9370이 IKEv2에서의 다중 키 교환을 규정한다.

## 연결

- [[MOC - Post-Quantum Cryptography]] 이 개념이 속한 PQC 도메인의 상위 지도이자 전이 전략의 분류 지점
- [[PQC 전이 감시]] 전이기 기본 배치를 하이브리드로 유지하도록 운영 기준을 두는 관리 영역
- [[Kyber (ML-KEM)]] 하이브리드의 PQC 측 구성 요소로 가장 널리 쓰이는 KEM 표준
- [[ECDH]] 하이브리드의 고전 측 구성 요소로 자주 결합되는 타원곡선 키 교환
- [[Crypto-Agility]] 하이브리드 협상과 알고리즘 교체를 가능케 하는 설계 전제
- [[Harvest Now Decrypt Later]] 하이브리드를 전이 전부터 서둘러 배치하게 만드는 소급 위협
- [[Shor's Algorithm]] 고전 측 구성 요소를 무력화해 PQC 병합을 필요하게 만드는 위협
- [[HKDF]] 두 비밀을 안전하게 병합하는 추출 기반 키 유도 함수
- [[X25519MLKEM768]] 실제 TLS에 배치된 대표적 하이브리드 키 교환 그룹
