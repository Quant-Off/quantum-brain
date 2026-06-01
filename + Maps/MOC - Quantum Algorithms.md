---
title: MOC - Quantum Algorithms
aliases: [양자 알고리즘, Quantum Algorithms, 양자 알고리즘 지도]
type: moc
status: budding
domain: quantum-algorithms
tags:
  - algorithm/shor
  - algorithm/grover
  - algorithm/qft
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
---

# MOC - Quantum Algorithms

## 개요
양자 알고리즘은 중첩과 간섭, 얽힘을 계산 자원으로 삼아 고전 알고리즘이 갖는 복잡도 한계를 넘으려는 절차다. 정수 인수분해와 이산로그를 다항 시간에 푸는 지수 가속부터 비구조적 탐색을 제곱근으로 줄이는 가속까지, 가속의 폭과 적용 범위가 문제 구조에 따라 갈린다. 이 지도는 대표 알고리즘과 그 골격을 이루는 핵심 서브루틴, 그리고 타겟이 되는 수학 문제로 가는 진입 경로를 모은다.

이 MOC는 알고리즘 도메인의 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할만 한다. 알고리즘 본체에서 출발해 위상 추정과 진폭 증폭 같은 공통 기법, 알고리즘이 환원하는 표적 문제, 그리고 인접 도메인 지도로 이어지도록 구조화한다.

## 핵심 알고리즘
- [[Shor's Algorithm]] 정수 인수분해와 이산로그를 다항 시간에 푸는 지수 가속 알고리즘
- [[Grover's Algorithm]] 비구조적 탐색을 약 $O(\sqrt{N})$로 가속하는 알고리즘
- [[Hamiltonian Simulation]] 양자계의 시간 발전을 효율적으로 모사하는 알고리즘

## 핵심 서브루틴과 기법
- [[Quantum Fourier Transform]] 주기성을 위상 정렬로 바꾸는 핵심 변환
- [[Quantum Phase Estimation]] 유니터리의 고윳값 위상을 추정하는 서브루틴
- [[Amplitude Amplification]] 해의 진폭을 키우는 Grover 반복의 일반화 기법
- [[Grover Diffusion Operator]] 평균 진폭 기준 반사를 수행하는 확산 연산자

## 표적 문제
- [[Discrete Logarithm Problem]] 쇼어가 인수분해와 동일 골격으로 푸는 이산로그 문제

## 관련 MOC
- [[MOC - Foundations of Quantum Information]] 상태와 측정 공준 등 토대 층
- [[MOC - Quantum Computing]] 큐비트와 게이트, 회로 등 알고리즘이 동작하는 계산 모형 층
- [[MOC - Post-Quantum Cryptography]] 쇼어가 무력화하는 공개키를 대체하는 양자 내성 암호 층
