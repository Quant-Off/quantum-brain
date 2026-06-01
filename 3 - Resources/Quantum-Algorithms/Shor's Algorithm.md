---
title: Shor's Algorithm
aliases: [쇼어 알고리즘, 쇼어, Shor, 쇼어의 알고리즘, Shor Factoring]
type: concept
status: budding
domain: quantum-algorithms
tags:
  - algorithm/shor
  - algorithm/qft
  - threat/harvest-now-decrypt-later
  - concept/superposition
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Quantum Algorithms]]"
related:
  - "[[MOC - Post-Quantum Cryptography]]"
  - "[[양자 위협 정세 감시]]"
  - "[[Quantum Superposition]]"
  - "[[Grover's Algorithm]]"
  - "[[Quantum Fourier Transform]]"
  - "[[Harvest Now Decrypt Later]]"
source:
  - "Shor, P. W. (1994), Algorithms for Quantum Computation: Discrete Logarithms and Factoring, FOCS"
  - "Shor, P. W. (1997), SIAM J. Comput. 26(5), DOI:10.1137/S0097539795293172"
confidence: high
---

# Shor's Algorithm

> 정수 인수분해와 이산로그를 고전 최선 알고리즘과 달리 다항 시간에 푸는 양자 알고리즘으로, RSA와 ECDH 같은 공개키 암호의 안전 근거를 직접 무너뜨린다.

## 핵심
쇼어 알고리즘은 큰 합성수 $N$을 소인수로 분해하는 문제를 위수 찾기(order finding) 문제로 환원한 뒤, 그 위수를 양자적으로 효율 추출하는 절차다. 핵심 통찰은 인수분해 자체가 본질이 아니라, $N$과 서로소인 임의의 밑 $a$에 대한 함수 $f(x) = a^{x} \bmod N$의 주기(위수) $r$, 즉 $a^{r} \equiv 1 \pmod{N}$를 만족하는 최소 양의 정수 $r$를 찾는 것이 본질이라는 점이다.

고전 부분은 다음과 같다. $r$이 짝수이고 $a^{r/2} \not\equiv -1 \pmod{N}$이면

$$ \gcd\!\left( a^{r/2} - 1,\, N \right) \quad\text{와}\quad \gcd\!\left( a^{r/2} + 1,\, N \right) $$

가운데 적어도 하나가 $N$의 자명하지 않은 인수를 준다. 임의로 고른 $a$가 이 조건을 만족할 확률이 충분히 높으므로, 몇 번의 시도로 인수를 얻는다. 어려운 단계는 위수 $r$ 자체를 찾는 것이고, 바로 이 지점에서 양자 우위가 발생한다.

양자 부분은 [[Quantum Superposition|중첩]]으로 모든 $x$ 값에 대한 $f(x)$를 한 번에 계산한 뒤, [[Quantum Fourier Transform|양자 푸리에 변환]](QFT)으로 함수의 주기성을 진폭에 새겨 넣는다. 측정하면 $r$의 정보를 담은 값이 높은 확률로 관측되고, 연분수 전개로 $r$을 복원한다. 고전적으로 주기를 찾으려면 지수적 비용이 들지만, QFT는 주기 구조를 위상 정렬로 바꾸어 간섭을 통해 효율적으로 드러낸다. 같은 골격으로 이산로그도 풀리므로 [[Discrete Logarithm Problem|이산로그 문제]] 기반 암호 또한 동일하게 위협받는다.

복잡도 측면에서 쇼어 알고리즘은 비트 수 $n = \log_2 N$에 대해 약 $\tilde{O}(n^{2})$ 게이트로 동작하여 $n$의 다항식 시간을 쓴다. 반면 고전 최선인 일반 수체 체(GNFS)는 준지수 시간

$$ \exp\!\left( \left( \tfrac{64}{9} \right)^{1/3} (\ln N)^{1/3} (\ln \ln N)^{2/3} \right) $$

를 요구한다. 다항과 준지수의 격차가 공개키 암호의 운명을 가른다.

## 흐름
```mermaid
flowchart TD
    A["밑 $$a$$ 무작위 선택, $$\gcd(a,N)=1$$ 확인"] --> B["양자: 중첩으로 $$f(x)=a^{x}\bmod N$$ 동시 계산"]
    B --> C["양자 푸리에 변환으로 주기 $$r$$ 정보를 진폭에 부호화"]
    C --> D["측정 후 연분수 전개로 위수 $$r$$ 복원"]
    D --> E{"$$r$$ 짝수이고 $$a^{r/2}\not\equiv -1$$?"}
    E -- 예 --> F["$$\gcd(a^{r/2}\pm 1, N)$$로 인수 추출"]
    E -- 아니오 --> A
```

## 왜 중요한가
쇼어 알고리즘은 양자컴퓨터가 단순한 가속기가 아니라 특정 수학 문제의 난이도 자체를 바꾼다는 사실을 보여 준 첫 사례다. RSA는 정수 인수분해의 난해성에, ECDH와 ECDSA와 DH는 이산로그의 난해성에 안전을 의탁하는데, 충분한 규모의 결함 허용 양자컴퓨터(CRQC)가 등장하면 이 두 가정이 동시에 붕괴한다. 그 결과 오늘날 인터넷 신뢰를 떠받치는 공개키 암호 계열은 강도 약화에 그치지 않고 사실상 전멸한다.

이 위협의 시점은 불확실하지만 대비는 지금 시작해야 한다. 공격자가 암호문을 미리 수집해 두었다가 CRQC가 준비되는 미래에 복호하는 [[Harvest Now Decrypt Later|선수집 후복호]] 전략 때문에, 장기 기밀이 필요한 데이터는 이미 오늘 위험에 노출되어 있다. 그래서 [[MOC - Post-Quantum Cryptography|양자 내성 암호]]로의 전이가 추진되고, 전이 시점 판단은 [[양자 위협 정세 감시|Mosca 부등식]]으로 정량화된다. 쇼어 알고리즘은 그 위협 모형에서 $Z$(CRQC 도래 시점)가 닥쳤을 때 공개키를 무너뜨리는 정확한 메커니즘에 해당한다.

같은 양자 알고리즘이라도 [[Grover's Algorithm|그로버 알고리즘]]은 대칭키와 해시의 강도를 제곱근만큼 깎는 데 그치는 반면, 쇼어는 공개키를 다항 시간에 완전히 파훼한다. 위협의 질이 다르다는 점이 양자 내성 암호 설계의 출발점이다.

## 연결
- [[Quantum Superposition]] 모든 입력에 대한 함수값을 한 번에 계산하는 양자 병렬성의 자원으로 쇼어의 양자 단계가 의존
- [[Quantum Fourier Transform]] 함수의 주기성을 위상 정렬로 바꾸어 위수를 효율 추출하는 핵심 도구
- [[Grover's Algorithm]] 같은 양자 알고리즘이지만 공개키 전멸이 아닌 대칭키 강도 약화에 그치는 대비 사례
- [[Discrete Logarithm Problem]] 인수분해와 더불어 동일 골격으로 풀려 ECDH와 DSA 계열을 위협하는 표적 문제
- [[MOC - Post-Quantum Cryptography]] 쇼어가 무력화하는 공개키를 대체하기 위한 표준과 전이 전략의 지도
- [[Harvest Now Decrypt Later]] CRQC 도래 이전에도 작동해 장기 기밀을 위협하는 소급 공격 모형
- [[양자 위협 정세 감시]] 쇼어가 작동할 CRQC 시점 $Z$를 입력으로 전이 시급성을 판단하는 책임 영역
