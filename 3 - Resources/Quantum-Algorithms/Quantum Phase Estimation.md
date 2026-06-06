---
title: Quantum Phase Estimation
aliases: [QPE, 양자 위상 추정, 위상 추정]
type: concept
status: budding
domain: quantum-algorithms
tags:
  - algorithm/qft
  - concept/operator
  - concept/measurement
  - math/linear-algebra
created: 2026-06-05
updated: 2026-06-05
up: "[[MOC - Quantum Algorithms]]"
related:
  - "[[Quantum Fourier Transform]]"
  - "[[Shor's Algorithm]]"
  - "[[Unitary Evolution]]"
  - "[[Observable (Hermitian Operator)]]"
source:
  - "Kitaev, A. Y. (1995), Quantum measurements and the Abelian Stabilizer Problem, arXiv:quant-ph/9511026"
  - "Nielsen, M. A. & Chuang, I. L. (2010), Quantum Computation and Quantum Information, Ch. 5"
confidence: high
---

# Quantum Phase Estimation

> 유니터리 연산자 $U$의 고유상태가 주어졌을 때 그에 대응하는 고유값의 위상 $\varphi$를 가산 레지스터에 이진 근사로 추출하는 양자 알고리즘으로, 양자 푸리에 변환을 거꾸로 적용해 위상을 측정 가능한 비트열로 바꾼다.

## 핵심
양자 위상 추정은 유니터리 연산자 $U$와 그 고유상태 $\lvert u \rangle$를 입력으로 받는다. 유니터리이므로 고유값은 항상 단위원 위에 놓이고, 따라서 어떤 실수 위상 $\varphi \in [0,1)$에 대해 다음을 만족한다.

$$ U \lvert u \rangle = e^{2\pi i \varphi} \lvert u \rangle $$

알고리즘의 목표는 직접 관측할 수 없는 이 위상 $\varphi$를 $t$비트 정밀도의 이진수로 읽어 내는 것이다. 측정으로는 전역 위상을 볼 수 없다는 [[Quantum Measurement|측정]]의 한계를 우회하기 위해, 위상을 상대 위상으로 바꾸고 다시 진폭으로 옮기는 두 단계를 거친다.

장치는 두 개의 레지스터로 구성된다. 첫째는 결과를 담을 $t$개의 카운팅 큐비트로 모두 $\lvert 0 \rangle$에서 시작해 각각에 아다마르 게이트를 걸어 균등 중첩을 만든다. 둘째는 고유상태 $\lvert u \rangle$를 담는 작업 레지스터다. 핵심은 제어 유니터리의 누적 적용이다. $j$번째 카운팅 큐비트가 $U^{2^{j}}$를 제어하도록 걸면, 위상 킥백(phase kickback)에 의해 고유값의 위상이 제어 큐비트로 되돌아온다. 이는 [[Unitary Evolution|유니터리 시간 발전]]을 $2^{j}$번 반복해 위상을 지수적으로 증폭하는 것에 해당한다.

제어 적용을 모두 마치면 카운팅 레지스터의 상태는 다음과 같이 위상이 부호화된 푸리에 기저 형태가 된다.

$$ \frac{1}{2^{t/2}} \sum_{k=0}^{2^{t}-1} e^{2\pi i \varphi k} \lvert k \rangle $$

이 상태는 정확히 $\varphi$를 주파수로 갖는 [[Quantum Fourier Transform|양자 푸리에 변환]]의 결과 모양이다. 따라서 역 양자 푸리에 변환 $\mathrm{QFT}^{\dagger}$를 적용하면 위상이 계산 기저의 한 점으로 모인다.

$$ \mathrm{QFT}^{\dagger} \left( \frac{1}{2^{t/2}} \sum_{k=0}^{2^{t}-1} e^{2\pi i \varphi k} \lvert k \rangle \right) \;\approx\; \lvert \widetilde{\varphi} \rangle $$

카운팅 레지스터를 측정하면 정수 $\widetilde{\varphi}$가 높은 확률로 관측되고, $\widetilde{\varphi} / 2^{t}$가 $\varphi$의 $t$비트 근사를 준다. $\varphi$가 $t$비트로 정확히 표현되면 측정은 결정적으로 정답을 주고, 그렇지 않으면 가장 가까운 근사가 가장 높은 확률을 차지한다. 정밀도를 한 비트 더 얻으려면 카운팅 큐비트를 하나 더 쓰면 되고, 실패 확률을 $\varepsilon$ 이하로 억제하려면 약 $t = n + \lceil \log_2(2 + 1/(2\varepsilon)) \rceil$비트를 둔다.

## 흐름
```mermaid
flowchart TD
    A["카운팅 레지스터 $$t$$큐비트를 $$\lvert 0 \rangle^{\otimes t}$$로, 작업 레지스터를 $$\lvert u \rangle$$로 초기화"] --> B["카운팅 큐비트마다 아다마르 적용해 균등 중첩 생성"]
    B --> C["$$j$$번째 큐비트로 $$U^{2^{j}}$$ 제어 적용, 위상 킥백으로 $$e^{2\pi i \varphi k}$$ 부호화"]
    C --> D["카운팅 레지스터에 역 푸리에 변환 $$\mathrm{QFT}^{\dagger}$$ 적용"]
    D --> E["카운팅 레지스터 측정으로 정수 $$\widetilde{\varphi}$$ 획득"]
    E --> F["$$\widetilde{\varphi}/2^{t}$$가 위상 $$\varphi$$의 $$t$$비트 근사"]
```

## 왜 중요한가
양자 위상 추정은 단일 알고리즘이라기보다 다른 여러 양자 알고리즘이 공유하는 핵심 부품이다. [[Shor's Algorithm|쇼어 알고리즘]]의 위수 찾기 단계는 곱셈 연산자의 고유값 위상을 추정하는 문제로 정확히 환원되며, 쇼어의 양자적 우위는 본질적으로 위상 추정이 주기를 효율적으로 드러내는 데서 나온다. 같은 맥락에서 이산로그 풀이, 숨은 부분군 문제, 행렬의 고유값을 다루는 HHL 선형계 풀이가 모두 위상 추정을 토대로 삼는다.

물리적 의미도 깊다. 해밀토니안 $H$의 시간 발전 연산자 $U = e^{-i H \tau}$의 고유값 위상은 곧 에너지 고유값에 비례하므로, 위상 추정은 분자나 물질의 바닥상태 에너지를 구하는 양자 화학 시뮬레이션의 표준 도구가 된다. 여기서 추정 대상 위상은 [[Observable (Hermitian Operator)|에르미트 연산자]]인 해밀토니안의 스펙트럼과 직접 연결되고, 측정할 수 없는 위상을 측정 가능한 비트열로 옮긴다는 점에서 양자 측정의 한계를 우회하는 정교한 설계가 드러난다.

다만 위상 추정은 정확한 고유상태 $\lvert u \rangle$를 미리 준비할 수 있어야 하고, 깊은 회로와 다수의 제어 유니터리, 정밀한 [[Quantum Fourier Transform|푸리에 변환]]을 요구한다. 이 자원 부담 때문에 결함 허용 양자컴퓨터(FTQC) 시대의 알고리즘으로 분류되며, 잡음이 큰 현재 장치에서는 반복 측정으로 자원을 줄인 반복 위상 추정이나 변분 기법 같은 대안이 함께 연구된다. 그럼에도 위상 추정은 양자컴퓨터가 고전적으로 어려운 스펙트럼 문제를 다항 자원으로 푸는 통로라는 점에서 양자 알고리즘 설계의 중심축으로 남아 있다.

## 연결
- [[Quantum Fourier Transform]] 위상이 부호화된 푸리에 기저 상태에 역변환을 적용해 위상을 계산 기저의 한 점으로 모으는 핵심 부품
- [[Shor's Algorithm]] 위수 찾기를 곱셈 연산자의 고유값 위상 추정으로 환원해 위상 추정을 양자적 우위의 엔진으로 사용하는 대표 응용
- [[Unitary Evolution]] 제어 유니터리를 $2^{j}$번 반복 적용하는 단계가 곧 위상을 지수적으로 증폭하는 시간 발전에 대응
- [[Observable (Hermitian Operator)]] 해밀토니안의 시간 발전 연산자에 적용하면 추정한 위상이 에너지 고유값에 대응하여 스펙트럼과 직접 연결
- [[Quantum Measurement]] 전역 위상을 관측할 수 없다는 측정의 한계를 상대 위상과 진폭으로 옮겨 우회하는 설계의 동기
