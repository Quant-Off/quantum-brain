---
title: MOC - Foundations of Quantum Information
aliases: [양자정보 기초, 양자정보과학 기초, Foundations of QIS]
type: moc
status: budding
domain: foundations
tags:
  - concept/formalism
  - concept/measurement
  - concept/entanglement
created: 2026-05-30
updated: 2026-05-30
up: ""
---

# MOC - Foundations of Quantum Information

## 개요
양자정보과학은 정보의 단위를 고전 비트가 아니라 양자 상태로 다룬다. 중첩과 얽힘, 측정의 확률적 붕괴 같은 양자역학의 공준이 정보 처리의 규칙 자체를 바꾸기 때문에, 고전 정보과학과는 표현력과 한계가 모두 달라진다. 이 지도는 그 토대가 되는 수학적 형식체계와 물리적 공준, 핵심 현상과 정리를 한자리에 모은다.

이 MOC는 도메인 최상위 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할만 한다. 상태를 기술하는 힐베르트 공간 형식에서 출발해 동역학과 측정 공준, 얽힘과 비국소성, 그리고 복제 불가 정리 같은 근본 한계까지 주제별로 진입 경로를 제공한다.

## 형식체계 (상태의 수학)
- [[Hilbert Space]] 양자 상태가 사는 복소 내적 공간
- [[Dirac Notation]] 켓과 브라로 상태와 연산을 적는 표기법
- [[Qubit]] 양자정보의 기본 단위
- [[Quantum Superposition]] 기저 상태의 선형 결합
- [[Tensor Product]] 복합계 상태 공간의 구성
- [[Density Matrix]] 혼합 상태까지 포괄하는 밀도 연산자 기술

## 상태의 기하와 연산자
- [[Bloch Sphere]] 단일 큐비트 상태의 기하학적 표현
- [[Pauli Matrices]] 큐비트 연산의 기본 연산자 집합

## 동역학과 측정 (공준)
- [[Unitary Evolution]] 닫힌 계의 결정적 시간 발전
- [[Observable (Hermitian Operator)]] 측정 가능량을 나타내는 에르미트 연산자
- [[Quantum Measurement]] 측정 공준과 상태 붕괴
- [[Born Rule]] 측정 결과의 확률 규칙
- [[Heisenberg Uncertainty Principle]] 비가환 관측량의 동시 결정 한계

## 얽힘과 비국소성
- [[Quantum Entanglement]] 부분계로 분리되지 않는 상관
- [[Bell States]] 최대 얽힘 두 큐비트 상태
- [[Bell Inequality (CHSH)]] 국소 실재론을 검증하는 부등식

## 핵심 정리와 한계
- [[No-Cloning Theorem]] 임의 미지 상태 복제 불가
- [[Quantum Decoherence]] 환경 결합에 의한 결맞음 상실

## 관련 MOC
- [[MOC - Quantum Information Theory]] 엔트로피와 채널 등 정보이론 층
- [[MOC - Quantum Computing]] 게이트와 회로 등 계산 모형 층
- [[MOC - Quantum Algorithms]] 알고리즘 설계와 복잡도, 서브루틴을 다루는 층
- [[MOC - Quantum Error Correction]] 결어긋남에 맞서는 보호 부호 층
- [[MOC - Quantum Cryptography]] 측정 교란과 복제 불가를 보안 근거로 쓰는 양자암호 층
