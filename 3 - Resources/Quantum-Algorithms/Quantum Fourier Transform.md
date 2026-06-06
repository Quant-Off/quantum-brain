---
title: Quantum Fourier Transform
aliases: [QFT, 양자 푸리에 변환, 양자 푸리에 변환(QFT)]
type: concept
status: budding
domain: quantum-algorithms
tags:
  - algorithm/qft
  - algorithm/shor
  - concept/gate
  - concept/superposition
  - math/linear-algebra
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Quantum Algorithms]]"
related:
  - "[[Shor's Algorithm]]"
  - "[[Quantum Phase Estimation]]"
  - "[[Hadamard Gate]]"
  - "[[Discrete Logarithm Problem]]"
source:
  - "Nielsen, M. A. & Chuang, I. L. (2010), Quantum Computation and Quantum Information, Ch. 5"
  - "Coppersmith, D. (1994), An Approximate Fourier Transform Useful in Quantum Factoring, IBM Research Report RC19642"
confidence: high
---

# Quantum Fourier Transform

> 고전 이산 푸리에 변환을 계산 기저 진폭에 적용하는 유니터리 연산으로, 약 $O(n^2)$ 게이트로 구현되어 쇼어 알고리즘과 위상 추정의 핵심 서브루틴이 된다.

## 핵심
양자 푸리에 변환은 이름은 같지만 고전 [[Discrete Fourier Transform|이산 푸리에 변환]](DFT)과 작동 방식이 다르다. 고전 DFT는 길이 $N$의 복소 벡터를 명시적으로 받아 또 다른 길이 $N$ 벡터로 내보내지만, QFT는 $N = 2^{n}$ 차원 상태의 진폭에 같은 변환을 한꺼번에 적용하는 $n$ 큐비트 유니터리다. 계산 기저 $\lvert j \rangle$에 대한 작용은 다음과 같다.

$$ \mathrm{QFT}\,\lvert j \rangle = \frac{1}{\sqrt{N}} \sum_{k=0}^{N-1} e^{2\pi i\, jk / N} \lvert k \rangle $$

따라서 임의의 상태 $\sum_{j} x_{j} \lvert j \rangle$는 진폭이 고전 DFT 계수 $y_{k}$로 바뀐 상태 $\sum_{k} y_{k} \lvert k \rangle$로 사상된다. 변환 자체는 진폭에 일어나므로 측정으로 모든 $y_{k}$를 직접 읽어 낼 수는 없다. QFT의 가치는 변환된 진폭을 출력하는 데 있지 않고, 주기성 같은 구조를 간섭으로 드러내 측정 확률에 새겨 넣는 데 있다.

핵심 통찰은 위 정의가 곱 형태(product form)로 인수분해된다는 점이다. $j$의 이진 전개 $j = j_1 j_2 \cdots j_n$와 이진 소수 표기 $0.j_l j_{l+1} \cdots j_n = \sum_{m} j_{l+m-1} / 2^{m}$을 쓰면 출력은 큐비트별 곱으로 분리된다.

$$ \mathrm{QFT}\,\lvert j_1 \cdots j_n \rangle = \frac{1}{\sqrt{N}} \bigotimes_{l=1}^{n} \left( \lvert 0 \rangle + e^{2\pi i\, (0.j_l j_{l+1} \cdots j_n)} \lvert 1 \rangle \right) $$

이 분리 덕분에 QFT를 두 종류의 게이트만으로 회로화할 수 있다. 각 큐비트에 [[Hadamard Gate|아다마르 게이트]] $H$를 걸어 기본 중첩과 첫 위상 비트를 만들고, 뒤따르는 큐비트들의 값에 조건부로 위상 회전 $R_k = \mathrm{diag}(1, e^{2\pi i / 2^{k}})$를 누적해 더 낮은 비트들을 위상에 채워 넣는다. 마지막으로 큐비트 순서를 뒤집는 스왑으로 곱 형태와 정의의 비트 순서를 맞춘다. $H$가 만드는 절반 위상에 제어 위상 회전이 나머지 자릿수를 합산하는 구조이므로, 아다마르 게이트가 단일 큐비트 QFT 그 자체라는 점이 이 회로의 출발점이다.

게이트 수는 첫 큐비트에 $H$ 하나와 제어 회전 $n-1$개, 다음 큐비트에 $H$ 하나와 제어 회전 $n-2$개 식으로 줄어들어 총 $n + \binom{n}{2} = O(n^2)$개다. 고전 고속 푸리에 변환(FFT)이 $O(N \log N) = O(n\, 2^{n})$ 연산을 쓰는 데 비해 QFT는 $n$의 다항식으로 동작한다. 다만 이 차수 격차를 곧바로 가속으로 등치할 수는 없다. QFT는 진폭을 변환할 뿐 그 값을 출력하지 않으므로, 실제 가속은 QFT를 주기 추출이나 위상 추정 같은 적절한 문제에 끼워 넣을 때만 발생한다.

## 구조
```mermaid
flowchart LR
    J["$$\lvert j_1 \cdots j_n \rangle$$"] --> H1["$$H$$ + 제어 위상 회전 $$R_2 \cdots R_n$$"]
    H1 --> H2["다음 큐비트 $$H$$ + $$R_2 \cdots R_{n-1}$$"]
    H2 --> DOTS["..."]
    DOTS --> SW["큐비트 순서 반전 스왑"]
    SW --> OUT["$$\frac{1}{\sqrt{N}} \bigotimes_{l} \left( \lvert 0 \rangle + e^{2\pi i (0.j_l \cdots j_n)} \lvert 1 \rangle \right)$$"]
```

## 왜 중요한가
QFT는 단독 알고리즘이라기보다 여러 지수 가속 알고리즘이 공유하는 엔진이다. [[Shor's Algorithm|쇼어 알고리즘]]에서는 함수 $f(x) = a^{x} \bmod N$를 중첩으로 계산한 뒤 QFT를 입력 레지스터에 적용한다. 그러면 주기 $r$를 가진 진폭들이 서로 보강 간섭하여 $r$의 역수에 가까운 위치에서 측정 확률이 높아지고, 연분수 전개로 위수를 복원한다. 고전적으로 주기 찾기는 지수 비용이 들지만, QFT가 주기 구조를 위상 정렬로 옮겨 간섭이 답을 드러내게 한다. 같은 골격이 [[Discrete Logarithm Problem|이산로그 문제]]에도 적용되므로 QFT는 공개키 암호 파훼 메커니즘의 심장부에 위치한다.

더 일반적으로 QFT는 [[Quantum Phase Estimation|양자 위상 추정]]의 마지막 단계다. 위상 추정은 유니터리 $U$의 고윳값 위상 $\varphi$를 보조 레지스터의 위상에 누적한 뒤 역 QFT를 적용해 $\varphi$의 이진 근사를 계산 기저로 읽어 낸다. 쇼어의 위수 찾기는 이 위상 추정의 특수한 경우로 볼 수 있다. 이렇게 QFT는 숨은 부분군 문제(hidden subgroup problem) 계열 전반을 관통하는 공통 부품으로, 양자 가속이 어디에서 어떻게 발생하는지를 이해하는 기준점이 된다.

실용적으로도 QFT는 다루기 좋다. 제어 위상 회전의 각도가 $2\pi / 2^{k}$로 빠르게 작아지므로, 작은 $k$의 회전만 남기고 나머지를 버리는 근사 QFT(approximate QFT)로 게이트 수를 $O(n \log n)$까지 줄이면서도 정확도 손실을 통제할 수 있다. 이는 잡음이 있는 실제 하드웨어에서 회로 깊이를 줄이는 핵심 기법이다.

## 연결
- [[Shor's Algorithm]] 함수의 주기성을 위상 정렬로 바꾸어 위수를 추출하는 양자 단계에서 QFT를 핵심 도구로 사용
- [[Quantum Phase Estimation]] 보조 레지스터에 누적된 고윳값 위상을 역 QFT로 계산 기저에 읽어 내는 상위 서브루틴
- [[Hadamard Gate]] 단일 큐비트 QFT 그 자체이자 QFT 회로의 큐비트마다 중첩과 첫 위상 비트를 만드는 기본 블록
- [[Discrete Logarithm Problem]] 쇼어가 동일 골격으로 푸는 표적 문제로, QFT의 주기 추출이 이산로그 파훼에도 그대로 적용
- [[Discrete Fourier Transform]] QFT가 진폭 위에서 구현하는 고전 변환으로, 차수 비교의 기준이 되는 원형
