---
title: MOC - Quantum Error Correction
aliases: [양자 오류정정, QEC, Quantum Error Correction MOC]
type: moc
status: evergreen
domain: quantum-error-correction
tags:
  - qec/stabilizer
  - qec/surface-code
  - qec/fault-tolerance
  - qec/passive-protection
  - concept/no-cloning
created: 2026-05-30
updated: 2026-06-05
up: "[[MOC - Foundations of Quantum Information]]"
---

# MOC - Quantum Error Correction

## 개요
양자 오류정정은 결어긋남과 게이트 오차로 손상되는 양자 정보를 다수의 물리 큐비트에 중복 부호화해 논리 큐비트로 보호하는 분야다. 고전 오류정정과 달리 복제 불가 정리 때문에 정보를 단순 복사할 수 없고, 비트 반전과 위상 반전이라는 연속적 오류 공간을 다뤄야 한다는 점에서 접근이 근본적으로 다르다. 핵심 발상은 데이터를 직접 측정하지 않고 안정자라 불리는 보조 관측량으로 신드롬만 추출해 오류 위치를 추정한 뒤 정정하는 것이다.

이 MOC는 양자 오류정정의 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할을 한다. 부호화의 일반 이론에서 출발해 대표적 부호인 표면 부호와 양자 LDPC 부호, 그리고 능동 정정 대신 잡음에 둔감한 부분공간에 정보를 묻는 수동적 보호 전략까지 주제별로 경로를 제공한다. 이제 오류를 안정자로 기술하는 형식론, 신드롬에서 회복 연산을 추정하는 복호, 임계값 아래에서 임의 길이 계산을 보장하는 내결함성 계층이 각각 개별 개념 노트로 갖춰졌으므로, 형식론에서 복호를 거쳐 내결함 논리 계산으로 이어지는 경로를 따라가며 학습할 수 있다.

## 핵심 개념
- [[Quantum Error Correction]] 물리 큐비트 중복 부호화로 논리 큐비트를 보호하는 일반 이론
- [[Pauli Matrices]] 비트 반전과 위상 반전 오류를 기술하는 기본 오류 연산자 집합
- [[Quantum Decoherence]] 환경 결합에 의한 결맞음 상실, 오류정정이 막으려는 일차 위협
- [[No-Cloning Theorem]] 정보 단순 복사를 금지해 고전식 중복화를 불가능하게 만드는 근본 한계

## 안정자 형식론
- [[Stabilizer Code]] 파울리 부분군의 +1 고유공간으로 부호공간을 정의하는 일반 틀
- [[Pauli Group]] 안정자와 오류를 담는 파울리 연산자 군
- [[Clifford Group]] 파울리 군을 보존하는 유니터리 군
- [[Logical Qubit]] 부호화된 추상 큐비트와 논리 연산자
- [[Syndrome Measurement]] 비파괴로 안정자만 측정해 오류 흔적을 얻는 절차
- [[Code Distance]] 논리 연산자 최소 무게로 정의되는 오류정정 능력
- [[Knill-Laflamme Conditions]] 오류 정정 가능성의 필요충분조건
- [[Gottesman-Knill Theorem]] 클리퍼드 회로를 고전으로 효율 모사할 수 있다는 정리

## 부호 (코드)
- [[Surface Code]] 평면 격자 위 안정자 부호, 높은 임계값과 국소 측정으로 유망한 후보
- [[CSS Code]] X형과 Z형 검사를 분리해 고전 부호 두 개로 구성하는 안정자 부호족
- [[LDPC Codes]] 희소 검사 행렬 기반 부호, 양자 LDPC로 부호화 효율을 끌어올리는 방향
- [[Quantum LDPC Code]] 희소 안정자로 일정 부호화율과 거리를 동시에 노리는 부호
- [[Tanner Graph]] 검사와 비트를 잇는 이분 그래프로 희소 부호 구조를 표현하는 도구
- [[Homology Class]] 위상 부호에서 논리 연산자를 위상적 비자명 사이클로 식별하는 분류

## 잡음 억제와 수동적 보호
- [[Decoherence-Free Subspace]] 집단 잡음에 둔감한 부분공간에 정보를 묻어 능동 정정 부담을 더는 전략
- [[Dynamical Decoupling]] 주기적 펄스 열로 환경 결합을 평균 소거하는 개루프 잡음 억제 기법

## 복호
- [[Decoder]] 신드롬에서 회복 연산을 추정하는 고전 알고리즘
- [[Minimum-Weight Perfect Matching]] 결함을 짝지어 오류 사슬을 찾는 표준 복호기
- [[Union-Find Decoder]] 거의 선형 시간 군집 성장 복호기
- [[Disjoint-Set Union]] 군집 병합을 거의 상수 시간에 처리하는 Union-Find 복호기의 핵심 자료구조

## 임계값의 통계역학 사상
- [[Random-Bond Ising Model]] 표면 부호 복호 임계값을 무질서 이징 모형의 상전이로 사상하는 모형
- [[Nishimori Line]] 무질서와 온도가 균형을 이루는 선으로 최적 복호 임계값에 대응하는 위치

## 내결함성과 논리 계산
- [[Quantum Threshold Theorem]] 임계값 아래에서 임의 길이 계산을 보장하는 정리
- [[Fault-Tolerant Quantum Computation]] 오류 연쇄 증식을 막고 논리 큐비트로 계산하는 틀
- [[Transversal Gate]] 블록별 독립 적용으로 결함 확산을 막는 논리 게이트
- [[Eastin-Knill Theorem]] 횡단 게이트만으로는 보편 집합을 이룰 수 없다는 불가능성 정리
- [[Magic State]] 횡단 게이트의 한계를 메우는 비클리퍼드 자원 상태
- [[Magic State Distillation]] 비클리퍼드 자원 상태를 정제해 보편 계산을 여는 기법

## 관련 MOC
- [[MOC - Foundations of Quantum Information]] 복제 불가와 결어긋남 등 토대 개념 층
- [[MOC - Quantum Hardware]] 물리 큐비트 구현과 오류율의 근원이 되는 하드웨어 층
- [[MOC - Quantum Computing]] 결함 허용 계산과 논리 큐비트를 소비하는 계산 모형 층
- [[양자 오류정정 문서화]] 안정자 형식론과 복호, 내결함성 개념 노트를 집필한 프로젝트 허브
