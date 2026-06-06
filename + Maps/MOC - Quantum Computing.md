---
title: MOC - Quantum Computing
aliases: [양자컴퓨팅, Quantum Computing, QC]
type: moc
status: budding
domain: quantum-computing
tags:
  - algorithm/shor
  - algorithm/grover
  - concept/superposition
created: 2026-05-30
updated: 2026-06-05
up: "[[MOC - Foundations of Quantum Information]]"
---

# MOC - Quantum Computing

## 개요
양자컴퓨팅은 중첩과 얽힘, 간섭을 계산 자원으로 동원해 고전 컴퓨터가 다루기 힘든 문제를 효율적으로 풀려는 계산 모형이다. 정보의 단위는 큐비트이고, 계산은 유니터리 게이트의 합성인 양자 회로로 표현되며, 마지막에 측정으로 고전 결과를 얻는다. 이 지도는 큐비트 모델과 게이트, 그리고 양자성을 실제 속도 이득으로 바꾸는 대표 알고리즘으로 가는 진입 경로를 모은다.

이 MOC는 양자컴퓨팅 도메인의 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할만 한다. 계산의 기본 단위와 상태에서 출발해 회로를 구성하는 게이트, 그 위에서 동작하는 핵심 알고리즘, 그리고 인접 도메인 지도로 이어지도록 구조화한다.

## 핵심 개념 (큐비트와 모델)
- [[Qubit]] 양자 계산의 기본 단위
- [[Quantum Superposition]] 병렬성의 근원이 되는 기저 상태의 선형 결합
- [[Unitary Evolution]] 양자 회로 연산을 떠받치는 닫힌 계의 결정적 발전
- [[Quantum Circuit]] 게이트의 합성으로 표현하는 계산 모형
- [[Quantum Measurement]] 계산 결과를 고전 비트로 읽어내는 단계

## 게이트와 회로
- [[Hadamard Gate]] 중첩을 만드는 단일 큐비트 게이트
- [[CNOT Gate]] 얽힘을 생성하는 2큐비트 제어 게이트
- [[Pauli Matrices]] 큐비트 연산의 기본 연산자 집합
- [[Universal Gate Set]] 임의 유니터리를 근사하는 게이트 집합

## 알고리즘
알고리즘 본체와 서브루틴, 표적 문제는 전용 지도 [[MOC - Quantum Algorithms]]에 정식으로 모은다. 이 지도에서는 계산 모형 위에서 양자성을 속도 이득으로 바꾸는 대표 사례만 진입점으로 둔다.
- [[Shor's Algorithm]] 정수 인수분해와 이산로그를 다항 시간에 푸는 알고리즘
- [[Grover's Algorithm]] 비정렬 탐색을 제곱근으로 가속하는 알고리즘

## 대안 계산 모형
- [[Measurement-Based Quantum Computing]] 얽힌 자원 상태에 적응적 측정만으로 계산하는 모형
- [[Adiabatic Quantum Computation]] 단열 정리를 따라 바닥 상태로 답을 인코딩하는 연속 시간 모형
- [[Quantum Annealing]] 단열 모형을 완화해 최적화 문제의 저에너지 해를 탐색하는 방식

## 계산 능력의 경계
- [[Quantum Supremacy]] 고전 계산으로 사실상 모사 불가능한 작업을 보이는 우월성 실증

## 관련 MOC
- [[MOC - Foundations of Quantum Information]] 상태와 측정 공준 등 토대 층
- [[MOC - Quantum Information Theory]] 엔트로피와 채널 등 정보이론 층
- [[MOC - Quantum Algorithms]] 알고리즘 설계와 복잡도를 다루는 층
- [[MOC - Quantum Error Correction]] 잡음에 맞서 논리 큐비트를 보호하는 층
- [[MOC - Quantum Hardware]] 큐비트를 물리적으로 구현하는 플랫폼 층
- [[MOC - Post-Quantum Cryptography]] Shor 위협에 대응하는 양자 내성 암호 층
