---
title: Decoy-State BB84
aliases: [디코이 상태 BB84, 디코이 상태 QKD, decoy-state QKD, decoy state]
type: concept
status: seedling
domain: quantum-cryptography
tags:
  - qkd/bb84
  - threat/eavesdropping-detection
  - concept/no-cloning
  - qkd/decoy-state
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Quantum Cryptography]]"
related:
  - "[[BB84 Protocol]]"
  - "[[Photon Number Splitting Attack]]"
  - "[[Quantum Key Distribution]]"
  - "[[Quantum Bit Error Rate (QBER)]]"
source:
  - "W.-Y. Hwang, Quantum Key Distribution with High Loss, Phys. Rev. Lett. 91, 057901 (2003)"
  - "H.-K. Lo, X. Ma, K. Chen, Decoy State Quantum Key Distribution, Phys. Rev. Lett. 94, 230504 (2005)"
confidence: high
---

# Decoy-State BB84

> 디코이 상태 BB84는 신호 펄스에 무작위로 세기가 다른 디코이 펄스를 섞어 보내고 세기별 통계를 비교함으로써, 약한 결맞음 펄스의 다광자 취약점을 노리는 [[Photon Number Splitting Attack|PNS 공격]]을 탐지하고 무력화하는 BB84 변형이다.

## 핵심
이상적인 [[BB84 Protocol|BB84]]는 펄스마다 정확히 광자 하나를 보내는 단일 광자원을 가정한다. 그러나 실제 송신기는 약한 결맞음 펄스(Weak Coherent Pulse, WCP)를 쓰며, 한 펄스에 들어가는 광자 수 $n$은 평균 광자 수 $\mu$를 모수로 하는 푸아송 분포를 따른다.

$$ P(n \mid \mu) = \frac{\mu^{n}}{n!}\, e^{-\mu} $$

이 때문에 일부 펄스는 광자를 둘 이상 담는다. [[Photon Number Splitting Attack|PNS 공격]]은 바로 이 다광자 펄스를 노린다. Eve는 다광자 펄스에서 광자 하나를 양자 메모리에 가로채 두고 나머지를 Bob에게 흘려 보낸다. 단일 광자 펄스는 통째로 차단해 손실로 위장하면, Eve는 흔적을 남기지 않고 사본을 확보한다. WCP만 쓰는 BB84의 안전 거리가 PNS로 크게 줄어드는 이유가 이것이다.

디코이 상태 기법(Hwang의 착상에서 출발)은 송신 측에 자유도 하나를 추가해 이 함정을 뒤집는다. Alice는 가변 광 감쇠기로 펄스마다 평균 광자 수를 무작위로 바꿔, 신호 세기 $\mu$와 하나 이상의 디코이 세기(예: 약한 디코이 $\nu$, 진공 디코이 $0$)를 섞어 보낸다. 전송과 측정이 끝난 뒤 Alice는 각 펄스가 어느 세기였는지를 공개 채널로 알린다. 핵심 비대칭은 다음이다. Eve의 공격은 펄스에 들어 있는 광자 수에만 작용할 뿐, 그 펄스가 신호였는지 디코이였는지는 구분하지 못한다.

따라서 광자 수 $n$이 같은 펄스라면, 그 펄스가 어느 세기 집합에서 나왔든 도청자가 보는 입장에서는 동일하다. 이 불변성에서 두 가지 세기 독립 결론이 따른다. $n$광자 펄스가 Bob에게 검출될 조건부 확률인 수율 $Y_n$과 그 오류율 $e_n$이 세기에 무관하다는 것이다.

$$ Y_n(\mu) = Y_n(\nu) = Y_n, \qquad e_n(\mu) = e_n(\nu) = e_n $$

세기 $\mu$에서 실제로 관측하는 양은 전체 게인 $Q_\mu$와 전체 [[Quantum Bit Error Rate (QBER)|QBER]] $E_\mu$이며, 이들은 광자 수별 기여의 푸아송 가중 합이다.

$$ Q_\mu = \sum_{n=0}^{\infty} \frac{\mu^{n}}{n!}\, e^{-\mu}\, Y_n, \qquad E_\mu Q_\mu = \sum_{n=0}^{\infty} \frac{\mu^{n}}{n!}\, e^{-\mu}\, e_n Y_n $$

여러 세기에서 $Q_\mu$와 $E_\mu$를 측정하면 위 식들은 미지수 $\{Y_n, e_n\}$에 대한 연립 제약이 된다. 이로부터 단일 광자 펄스의 수율 $Y_1$과 오류율 $e_1$의 하한과 상한을 정직하게 추정할 수 있다. PNS 공격자는 단일 광자 펄스를 골라 차단하므로 $Y_1$을 비정상적으로 끌어내리는데, 디코이 세기와 신호 세기의 통계를 동시에 일관되게 흉내낼 수 없어 이 조작이 $Y_1$ 추정에서 드러난다. 결과적으로 단일 광자 기여만으로 만들어지는 비밀 키 성분을 따로 떼어내 보안 키율을 계산할 수 있고, 사실상 단일 광자원에 가까운 키율과 전송 거리가 회복된다.

비밀 키율은 [[Quantum Bit Error Rate (QBER)|QBER]] 정보와 [[Photon Number Splitting Attack|PNS]] 내성을 결합한 GLLP 형태의 하한으로 표현된다. 단일 광자 성분의 검출 비율을 $\Omega = Y_1 \mu e^{-\mu} / Q_\mu$로 두면 키율은 대략 다음과 같다.

$$ R \;\gtrsim\; q\,\big[\, -Q_\mu f(E_\mu)\, H_2(E_\mu) + \Omega\, Q_\mu\,(1 - H_2(e_1)) \,\big] $$

여기서 $H_2(x) = -x\log_2 x - (1-x)\log_2(1-x)$는 이진 섀넌 엔트로피, $f(E_\mu)$는 오류 정정 효율 인자, $q$는 프로토콜 효율 상수다. 디코이 추정으로 $Y_1$과 $e_1$의 빠듯한 경계를 얻을수록 둘째 항의 안전한 단일 광자 키 기여가 커지고, 키율이 채널 투과율 $\eta$에 대해 선형($O(\eta)$)에 가깝게 거동한다. 디코이 없이 최악의 경우를 가정하면 같은 양이 $O(\eta^2)$로 떨어진다.

## 흐름
```mermaid
flowchart TD
    A["Alice: 세기 무작위 선택\n신호 $$\mu$$, 디코이 $$\nu$$, 진공 $$0$$"] --> B[양자 채널 전송]
    E["Eve: 광자 수에만 작용\n(세기 구분 불가)"] -. PNS 시도 .-> B
    B --> C[Bob 측정]
    C --> D[Alice가 세기 사후 공개]
    D --> F["세기별 게인 $$Q_\mu$$, $$E_\mu$$ 산출"]
    F --> G["수율 불변성으로\n$$Y_1$$, $$e_1$$ 추정"]
    G --> K["GLLP 보안 키율 $$R$$"]
```

## 왜 중요한가
PNS 공격은 약한 결맞음 펄스를 쓰는 모든 준비-측정형 QKD의 실용 보안과 도달 거리를 근본적으로 깎아내린 위협이었다. 디코이 상태 기법은 송신기에 세기 무작위화라는 값싼 자유도 하나만 더해, 단일 광자원 없이도 단일 광자 통계를 정직하게 추정하는 길을 열었다. 그 결과 WCP 기반 BB84의 안전 거리와 키율을 단일 광자원에 근접한 수준으로 복원했고, GLLP 보안 분석과 결합되어 무조건적 안전성을 실제 하드웨어에서 주장할 수 있게 했다. 추가 광원이나 특수 검출기가 필요 없고 기존 BB84 송신단에 감쇠기 변조만 얹으면 된다는 점 때문에, 디코이 상태 BB84는 오늘날 현장 QKD 실장의 사실상 표준 기법으로 자리 잡았다.

## 연결
- [[BB84 Protocol]] 디코이 상태 BB84가 세기 무작위화를 더해 확장하는 기반 프로토콜
- [[Photon Number Splitting Attack|PNS 공격]] 디코이 기법이 탐지하고 무력화하는 다광자 취약점 공격
- [[Quantum Key Distribution]] 이 변형이 실용 보안 거리를 회복시키는 상위 키 분배 틀
- [[Quantum Bit Error Rate (QBER)|QBER]] 세기별로 비교해 단일 광자 오류율 $e_1$을 분리 추정하는 핵심 관측량
