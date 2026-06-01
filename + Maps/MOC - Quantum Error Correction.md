---
title: MOC - Quantum Error Correction
aliases: [양자 오류정정, QEC, Quantum Error Correction MOC]
type: moc
status: budding
domain: quantum-error-correction
tags:
  - qec/stabilizer
  - qec/surface-code
  - concept/no-cloning
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
---

# MOC - Quantum Error Correction

## 개요
양자 오류정정은 결어긋남과 게이트 오차로 손상되는 양자 정보를 다수의 물리 큐비트에 중복 부호화해 논리 큐비트로 보호하는 분야다. 고전 오류정정과 달리 복제 불가 정리 때문에 정보를 단순 복사할 수 없고, 비트 반전과 위상 반전이라는 연속적 오류 공간을 다뤄야 한다는 점에서 접근이 근본적으로 다르다. 핵심 발상은 데이터를 직접 측정하지 않고 안정자라 불리는 보조 관측량으로 신드롬만 추출해 오류 위치를 추정한 뒤 정정하는 것이다.

이 MOC는 양자 오류정정의 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할을 한다. 부호화의 일반 이론에서 출발해 대표적 부호인 표면 부호와 양자 LDPC 부호, 그리고 능동 정정 대신 잡음에 둔감한 부분공간에 정보를 묻는 수동적 보호 전략까지 주제별로 경로를 제공한다.

## 핵심 개념
- [[Quantum Error Correction]] 물리 큐비트 중복 부호화로 논리 큐비트를 보호하는 일반 이론
- [[Pauli Matrices]] 비트 반전과 위상 반전 오류를 기술하는 기본 오류 연산자 집합
- [[Quantum Decoherence]] 환경 결합에 의한 결맞음 상실, 오류정정이 막으려는 일차 위협
- [[No-Cloning Theorem]] 정보 단순 복사를 금지해 고전식 중복화를 불가능하게 만드는 근본 한계

## 부호 (코드)
- [[Surface Code]] 평면 격자 위 안정자 부호, 높은 임계값과 국소 측정으로 유망한 후보
- [[LDPC Codes]] 희소 검사 행렬 기반 부호, 양자 LDPC로 부호화 효율을 끌어올리는 방향

## 잡음 억제와 수동적 보호
- [[Decoherence-Free Subspace]] 집단 잡음에 둔감한 부분공간에 정보를 묻어 능동 정정 부담을 더는 전략

## 관련 MOC
- [[MOC - Foundations of Quantum Information]] 복제 불가와 결어긋남 등 토대 개념 층
- [[MOC - Quantum Hardware]] 물리 큐비트 구현과 오류율의 근원이 되는 하드웨어 층
- [[MOC - Quantum Computing]] 결함 허용 계산과 논리 큐비트를 소비하는 계산 모형 층
