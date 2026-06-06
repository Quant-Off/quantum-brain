---
title: WOTS+
aliases: [WOTS+, Winternitz OTS, 윈터니츠 일회용 서명, Winternitz One-Time Signature, W-OTS+]
type: concept
status: budding
domain: post-quantum-cryptography
tags:
  - pqc/signature
  - pqc/hash-based
  - nist/fips-205
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Post-Quantum Cryptography]]"
related:
  - "[[SPHINCS+ (SLH-DSA)]]"
  - "[[XMSS]]"
  - "[[FORS]]"
  - "[[Hypertree]]"
  - "[[Grover's Algorithm]]"
source:
  - "NIST FIPS 205"
  - "RFC 8391"
confidence: high
---

# WOTS+

> 단 하나의 메시지에만 안전하게 쓸 수 있는 해시 기반 일회용 서명 방식으로, 하나의 키 쌍으로 한 번만 서명한다는 제약 대신 충돌 저항성에 의존하지 않는 보수적 안전성을 얻는다.

## 핵심
WOTS+는 일방향 함수의 반복 적용, 즉 해시 체인 위에서 동작하는 일회용 서명(One-Time Signature)이다. 이름의 윈터니츠는 메시지 비트를 한 번에 하나씩 다루는 대신, $w$진수 자릿값 단위로 묶어 서명 크기와 연산량을 맞바꾸는 발상에서 왔다. 매개변수 $w$를 키우면 체인이 길어져 서명은 짧아지지만 해시 호출이 늘고, $w$를 줄이면 반대가 된다.

서명할 메시지 다이제스트는 길이 $n$ 비트이며, 이를 밑 $w$로 표현해 $\ell_1$개의 자릿값으로 쪼갠다. 여기에 위조를 막는 체크섬 자릿값 $\ell_2$개를 덧붙여 총

$$ \ell = \ell_1 + \ell_2, \qquad \ell_1 = \left\lceil \frac{n}{\log_2 w} \right\rceil, \qquad \ell_2 = \left\lfloor \frac{\log_2(\ell_1(w-1))}{\log_2 w} \right\rfloor + 1 $$

개의 자릿값을 얻는다. 비밀키는 이 $\ell$개 자리 각각에 대응하는 무작위 길이 $n$ 비트 값 $(\mathsf{sk}_1, \dots, \mathsf{sk}_\ell)$이다. 공개키는 각 비밀키에 일방향 함수 $F$를 정확히 $w-1$번 적용한 체인의 끝값이다. 즉 자리 $i$에서 길이 $k$의 체인은 다음과 같이 정의된다.

$$ c^{0}(x) = x, \qquad c^{k}(x) = F\big(c^{k-1}(x) \oplus \mathbf{r}_k\big) $$

여기서 비트마스크 $\mathbf{r}_k$를 매 단계 XOR하는 점이 고전 WOTS와 갈라지는 핵심이다. 이 마스킹 덕분에 안전성 증명이 해시 함수의 충돌 저항성이 아니라 더 약한 가정인 제2 원상 저항성과 원상 저항성에 환원되며, 그래서 표기에 더하기(+)가 붙는다.

서명은 각 자릿값 $b_i$만큼 비밀키에서 체인을 전진시킨 중간값을 묶은 것이다.

$$ \sigma_i = c^{b_i}(\mathsf{sk}_i), \qquad \sigma = (\sigma_1, \dots, \sigma_\ell) $$

검증자는 각 $\sigma_i$에서 남은 $w-1-b_i$번의 체인 적용을 마저 수행해 끝값을 재구성하고, 그 묶음이 공개키와 일치하는지 확인한다. 체크섬 자릿값이 핵심 방어선이다. 공격자가 어떤 메시지 자릿값을 올리면(체인을 더 전진시킬 수 있으면) 체크섬 자릿값은 반드시 내려가야 하는데, 체인은 한 방향으로만 전진하므로 내려간 값을 위조할 수 없다. 이 구조 때문에 같은 키로 두 번 서명하면 공격자가 두 서명을 조합해 새 메시지의 서명을 합성할 수 있어, 일회용이라는 제약이 절대적이다.

## 구조
```mermaid
flowchart LR
    SK["$$\mathsf{sk}_i$$ 비밀키 자리"] -->|"$$c^{b_i}$$"| SIG["$$\sigma_i$$ 서명 조각"]
    SIG -->|"남은 $$w-1-b_i$$ 회 체인"| PK["$$\mathsf{pk}_i$$ 공개키 끝값"]
    M["메시지 다이제스트 $$n$$비트"] --> D["밑 $$w$$ 자릿값 $$\ell_1$$개"]
    D --> CS["체크섬 자릿값 $$\ell_2$$개"]
    D --> SIG
    CS --> SIG
```

## 왜 중요한가
WOTS+ 하나로는 메시지 한 건만 서명할 수 있으니 실용성이 없어 보이지만, 해시 기반 서명 체계의 가장 작은 빌딩 블록으로서 결정적인 역할을 한다. [[XMSS]]는 다수의 WOTS+ 공개키를 머클 트리의 잎으로 삼아 묶고 하나의 트리 루트로 응축함으로써, 수많은 일회용 키를 단일 공개키 뒤에 숨긴다. [[SPHINCS+ (SLH-DSA)|SPHINCS+]]는 이 XMSS 트리들을 다시 [[Hypertree]]로 계층화하고, 메시지마다 쓸 잎을 [[FORS]]로 무상태 선택해, 상태 관리 없이도 사실상 무제한 서명을 가능하게 한다. 다시 말해 WOTS+는 NIST가 FIPS 205로 표준화한 SLH-DSA의 잎 수준 서명 원시 요소다.

이 방식의 가치는 보수성에 있다. 안전성이 격자나 코드 같은 구조화된 수학 가정이 아니라 오직 해시 함수의 일방향성에서 나오므로, 다른 PQC 가정이 흔들려도 견디는 수학적 백업이 된다. 양자 공격자가 [[Grover's Algorithm|그로버 알고리즘]]으로 일방향 함수의 원상을 탐색해도 비용은 $O(2^{n/2})$ 수준에 그치며, 매개변수 $n$은 이 제곱근 가속을 흡수하도록 잡는다. 대가는 체인 길이에 비례하는 해시 호출과 $\ell$개 조각으로 불어나는 서명 크기이며, 이 크기 문제 때문에 WOTS+는 단독이 아니라 항상 트리 구조와 함께 배치된다.

## 연결
- [[MOC - Post-Quantum Cryptography]] 이 개념이 속한 도메인 지도이자 상위 진입점
- [[SPHINCS+ (SLH-DSA)]] WOTS+를 잎 수준 일회용 서명으로 삼는 무상태 해시 기반 서명 표준
- [[XMSS]] 다수의 WOTS+ 공개키를 머클 트리로 묶어 한 루트로 응축하는 상태 보유형 서명
- [[Hypertree]] XMSS 트리들을 계층화해 단일 키로 다수 서명을 인증하는 상위 구조
- [[FORS]] 메시지마다 쓸 잎(WOTS+ 키)을 무상태로 고르는 few-time 서명 선택 메커니즘
- [[Grover's Algorithm]] 해시 기반 서명의 보안 강도 매개변수 $n$을 $O(2^{n/2})$로 결정짓는 양자 공격
