---
title: FORS
aliases: [FORS, Forest of Random Subsets, 무작위 부분집합 숲]
type: concept
status: budding
domain: post-quantum-cryptography
tags:
  - pqc/signature
  - pqc/hash-based
  - nist/fips-205
  - concept/formalism
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Post-Quantum Cryptography]]"
related:
  - "[[SPHINCS+ (SLH-DSA)]]"
  - "[[WOTS+]]"
  - "[[Hypertree]]"
  - "[[XMSS]]"
source:
  - "NIST FIPS 205"
confidence: high
---

# FORS

> 같은 키 쌍으로 정해진 횟수까지 안전하게 서명할 수 있는 해시 기반 소수 회(few-time) 서명 방식으로, SPHINCS+에서 메시지 다이제스트를 서명하는 최하층 구성요소다.

## 핵심
FORS는 Forest of Random Subsets, 즉 무작위 부분집합의 숲이라는 이름 그대로 다수의 작은 머클 트리를 병렬로 묶은 구조다. 일회용 서명이 키 한 쌍으로 단 한 번만 안전한 것과 달리, FORS는 같은 키 쌍으로 통계적으로 정해진 소수 회까지 서명을 견디도록 설계된 few-time 서명(FTS)이다. [[SPHINCS+ (SLH-DSA)]]에서 실제로 메시지를 서명하는 부분이 바로 FORS이며, 그 위층인 [[Hypertree]]와 [[WOTS+]]는 FORS 공개키를 다시 인증하는 역할만 맡는다.

매개변수는 두 정수 $k$와 $t = 2^a$로 정해진다. FORS 키는 $k$개의 트리로 구성되고, 각 트리는 $t$개의 잎(leaf)을 가진다. 각 잎 아래에는 무작위로 생성한 비밀 값이 하나씩 있으므로, 비밀 키는 총 $k \cdot t$개의 비밀 값 집합이다. 잎은 해당 비밀 값의 해시이고, 잎들을 머클 트리로 쌓아 올린 각 트리의 루트 $k$개를 다시 한 번 해시해 압축한 값이 FORS 공개키가 된다.

$$ pk_{\text{FORS}} = \mathsf{H}\big(\, \mathsf{root}_0 \,\Vert\, \mathsf{root}_1 \,\Vert\, \cdots \,\Vert\, \mathsf{root}_{k-1} \,\big) $$

서명은 메시지 다이제스트를 $k$개의 인덱스로 잘라 읽는 것에서 시작한다. $ka$ 비트짜리 다이제스트를 $a$ 비트씩 $k$ 조각으로 나누면, $i$번째 조각이 $i$번째 트리에서 공개할 잎의 위치 $idx_i \in \{0, \dots, t-1\}$를 가리킨다. 서명자는 각 트리마다 그 위치에 해당하는 비밀 값 하나와, 그 잎에서 루트까지 이어지는 인증 경로(authentication path)를 함께 공개한다. 즉 메시지가 $k$개의 비밀 값으로 이루어진 부분집합을 무작위로 선택하는 셈이고, 여기서 무작위 부분집합의 숲이라는 이름이 나온다.

검증자는 공개된 각 비밀 값과 인증 경로로 $k$개의 트리 루트를 재계산하고, 이를 압축해 얻은 값이 인증된 FORS 공개키와 같은지 확인한다.

$$ \mathrm{Verify}_{\text{FORS}} = 1 \iff \mathsf{H}\big(\, \mathsf{root}'_0 \,\Vert\, \cdots \,\Vert\, \mathsf{root}'_{k-1} \,\big) = pk_{\text{FORS}} $$

few-time 안전성의 근거는 한 번의 서명이 각 트리에서 잎 하나씩만 드러낸다는 데 있다. 서명을 거듭할수록 노출된 비밀 값이 쌓이지만, 위조하려면 공격자가 타겟 메시지가 가리키는 $k$개 위치의 비밀 값을 모두 이미 본 적이 있어야 한다. 위조 성공 확률은 서명 횟수가 늘수록 증가하므로, $k$와 $t$는 상위 [[Hypertree]]가 한 FORS 키를 재사용하게 되는 횟수까지 고려해 위조 확률을 무시할 만한 수준으로 묶도록 정한다.

## 구조
```mermaid
flowchart TD
    MD["메시지 다이제스트 ($$ka$$ 비트)"] --> SP["$$a$$ 비트씩 $$k$$ 조각으로 분할"]
    SP --> I0["인덱스 $$idx_0$$"]
    SP --> Ik["인덱스 $$idx_{k-1}$$"]
    I0 --> T0["트리 0: 잎 $$idx_0$$의 비밀 값 + 인증 경로"]
    Ik --> Tk["트리 $$k-1$$: 잎 $$idx_{k-1}$$의 비밀 값 + 인증 경로"]
    T0 --> R0["$$\mathsf{root}_0$$"]
    Tk --> Rk["$$\mathsf{root}_{k-1}$$"]
    R0 --> PK["$$pk_{\text{FORS}} = \mathsf{H}(\mathsf{root}_0 \Vert \cdots \Vert \mathsf{root}_{k-1})$$"]
    Rk --> PK
```

## 왜 중요한가
SPHINCS+가 무상태(stateless) 서명일 수 있는 핵심 열쇠가 FORS다. 머클 트리 기반 서명에서 가장 까다로운 문제는 일회용 키를 두 번 쓰지 않도록 사용한 인덱스를 추적하고 갱신하는 것이고, 이 상태 관리가 [[XMSS]]나 LMS 같은 상태 기반(stateful) 방식의 실용적 약점이다. 백업 복원이나 가상머신 복제로 상태가 되감기면 같은 키가 재사용되어 보안이 무너진다. SPHINCS+는 메시지마다 FORS 키 하나를 의사난수로 골라 쓰고, FORS가 소수 회 재사용을 견디도록 설계되어 있어서, 약간의 우연한 재사용이 일어나도 위조 확률이 안전 한계 안에 머문다. 덕분에 서명자가 어떤 상태도 들고 다니지 않아도 된다.

또한 FORS는 SPHINCS+ 전체의 보수적 보안 가정을 유지하는 데 기여한다. 충돌 저항성에 의존하는 대신 (다중 타겟) 제2 원상 저항성 같은 더 약하고 잘 연구된 해시 성질에서 안전성을 끌어내도록 설계되어, [[SPHINCS+ (SLH-DSA)]] 전체가 해시 함수 하나의 안전성만을 신뢰 근거로 삼는 그림을 완성한다. 메시지를 서명하는 최하층이 이런 보수적 가정 위에 서 있어야, 그 위를 인증하는 [[WOTS+]]와 [[Hypertree]]를 합친 구조 전체의 안전성 주장이 성립한다.

## 연결
- [[SPHINCS+ (SLH-DSA)]] FORS를 최하층 few-time 서명으로 품어 무상태 서명을 완성하는 상위 구성
- [[WOTS+]] FORS 공개키를 다시 인증하는 일회용 서명이며 한 단계 위층 구성요소
- [[Hypertree]] FORS 공개키를 머클 트리 계층으로 묶어 최상위 루트까지 잇는 인증 구조
- [[XMSS]] 상태를 들고 다녀야 하는 상태 기반 서명으로, FORS의 무상태 설계와 대비되는 형제 방식
- [[MOC - Post-Quantum Cryptography]] 이 개념이 속한 도메인 지도이자 상위 진입점
