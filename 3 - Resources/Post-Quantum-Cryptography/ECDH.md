---
title: ECDH
aliases: [타원곡선 디피-헬먼, Elliptic-Curve Diffie-Hellman, ECDH, 타원곡선 키 교환, X25519]
type: concept
status: budding
domain: post-quantum-cryptography
tags:
  - migration/hybrid
  - threat/harvest-now-decrypt-later
  - algorithm/shor
  - math/linear-algebra
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Post-Quantum Cryptography]]"
related:
  - "[[Hybrid Key Exchange]]"
  - "[[Shor's Algorithm]]"
  - "[[Discrete Logarithm Problem]]"
  - "[[X25519MLKEM768]]"
source:
  - "RFC 7748 (Curve25519, X25519)"
  - "NIST SP 800-186"
  - "Diffie, W. and Hellman, M. (1976), New Directions in Cryptography, IEEE Trans. Inf. Theory"
confidence: high
---

# ECDH

> 타원곡선 위의 점 덧셈이 만드는 군에서 양쪽이 각자의 비밀 스칼라로 같은 점을 곱해 공유 비밀에 도달하는 고전 키 합의 방식이다.

## 핵심

ECDH는 디피-헬먼 키 합의를 곱셈군 대신 타원곡선이 이루는 덧셈군 위에서 수행한다. 공개 파라미터로 곡선과 생성원 점 $G$가 고정되어 있을 때, 두 참여자는 각자 비밀 스칼라 $a$와 $b$를 무작위로 뽑고 공개값으로 $A = aG$와 $B = bG$를 계산해 교환한다. 곡선 위의 스칼라 곱은 결합법칙과 교환법칙을 만족하므로 양쪽이 같은 점에 도달한다.

$$ S = a(bG) = b(aG) = abG $$

이렇게 얻은 공유 점 $S$의 한 좌표를 키 유도 함수에 넣어 대칭 세션 키를 만든다. 수동 도청자는 곡선, $G$, $A$, $B$만 관측하므로, 비밀을 복원하려면 $A = aG$에서 스칼라 $a$를 역산해야 한다. 이것이 타원곡선판 [[Discrete Logarithm Problem|이산로그 문제]](ECDLP)이며, 고전 알고리즘으로는 다항 시간 풀이가 알려져 있지 않다.

실무에서 가장 널리 쓰이는 구현은 Curve25519 위의 X25519다. 몽고메리 곡선 형태를 써서 한 좌표만으로 안전하고 빠르게 스칼라 곱을 계산하고, 측면 채널에 강한 상수 시간 구현이 용이하다. 같은 보안 강도를 RSA보다 훨씬 짧은 키 길이로 달성한다는 점이 ECDH 계열이 TLS와 SSH, Signal 같은 프로토콜의 기본 키 교환으로 자리 잡은 이유다.

## 흐름

```mermaid
sequenceDiagram
    participant A as Alice
    participant B as Bob
    Note over A,B: 공개 파라미터 곡선과 생성원 $$G$$
    A->>A: 비밀 $$a$$ 선택, 공개 $$A = aG$$
    B->>B: 비밀 $$b$$ 선택, 공개 $$B = bG$$
    A->>B: $$A$$ 전송
    B->>A: $$B$$ 전송
    A->>A: $$S = aB = abG$$
    B->>B: $$S = bA = abG$$
    Note over A,B: 공유 비밀 $$S$$ 에서 세션 키 유도
```

## 왜 중요한가

ECDH의 안전성은 ECDLP의 고전적 난해함이라는 단 하나의 가정 위에 서 있다. 문제는 이 가정이 양자 공격에 무너진다는 점이다. [[Shor's Algorithm|쇼어 알고리즘]]은 충분히 큰 결함 허용 양자컴퓨터에서 이산로그를 다항 시간에 풀고, 타원곡선군의 이산로그도 예외가 아니다. 따라서 ECDH 단독으로는 양자 시대에 기밀성을 보장할 수 없다.

당장 양자컴퓨터가 없더라도 위협은 현재 진행형이다. 공격자가 오늘 암호문과 키 교환 트랜스크립트를 수집해 두었다가 미래에 복호화하는 수확 후 복호화(Harvest Now Decrypt Later) 시나리오 때문에, 장기 기밀이 필요한 데이터는 지금 보호되어야 한다.

그럼에도 ECDH는 폐기 대상이 아니라 전이기의 한 축으로 재배치된다. [[Hybrid Key Exchange|하이브리드 키 교환]]은 ECDH가 만든 고전 비밀과 격자 기반 KEM이 만든 양자 내성 비밀을 함께 유도 함수에 넣어 병합한다. 이 구성에서 ECDH는 수십 년간 검증된 고전 절반을 담당하고, 새 PQC 알고리즘에 아직 알려지지 않은 결함이 있더라도 안전망 역할을 한다. [[X25519MLKEM768]]이 바로 X25519 형태의 ECDH와 ML-KEM-768을 묶은 표준 하이브리드 조합이다.

## 연결

- [[Discrete Logarithm Problem]] ECDH의 안전성이 의존하는 근본 난제로, 타원곡선군에서는 ECDLP 형태를 띤다
- [[Shor's Algorithm]] ECDLP를 다항 시간에 풀어 ECDH를 양자적으로 무력화하는 알고리즘
- [[Hybrid Key Exchange]] ECDH가 고전 절반을 맡아 PQC KEM과 비밀을 병합하는 전이기 배치 방식
- [[X25519MLKEM768]] X25519 형태의 ECDH와 ML-KEM-768을 결합한 구체적 하이브리드 표준 조합
- [[Kyber (ML-KEM)]] 하이브리드에서 ECDH와 짝을 이루는 격자 기반 양자 내성 KEM
- [[Harvest Now Decrypt Later]] ECDH 트랜스크립트를 지금 수확해 미래에 복호화하려는 위협 모델
