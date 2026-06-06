---
title: MOC - Quantum Hardware
aliases: [양자 하드웨어, Quantum Hardware, 큐비트 물리 구현]
type: moc
status: budding
domain: quantum-hardware
tags:
  - hardware/superconducting
  - hardware/trapped-ion
  - hardware/photonic
created: 2026-05-30
updated: 2026-06-05
up: "[[MOC - Foundations of Quantum Information]]"
---

# MOC - Quantum Hardware

## 개요
양자 하드웨어는 추상적인 큐비트를 실제 물리계로 구현하는 층이다. 어떤 물리계를 큐비트로 쓰느냐에 따라 결맞음 시간, 게이트 충실도, 연결성, 확장성, 동작 온도가 전혀 달라지므로, 알고리즘과 오류정정 설계의 전제가 되는 토대다. 이 지도는 대표적인 큐비트 물리 구현 방식과 그 공통 한계를 한자리에 모은다.

이 MOC는 양자 하드웨어 도메인의 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할을 한다. 큐비트라는 정보 단위에서 출발해 초전도, 이온트랩, 광자 같은 구현 플랫폼으로 들어가고, 모든 구현이 함께 맞서는 결어긋남 문제로 이어지는 진입 경로를 제공한다.

## 핵심 개념
- [[Qubit]] 모든 물리 구현이 실체화하려는 양자정보의 기본 단위

## 큐비트 물리 구현 플랫폼
- [[Superconducting Qubit]] 조셉슨 접합 기반 초전도 회로 큐비트
- [[Trapped-Ion Qubit]] 전자기 트랩에 가둔 이온의 내부 준위 큐비트
- [[Photonic Qubit]] 광자의 편광이나 경로 자유도를 쓰는 큐비트

## 소자와 구성 요소
- [[Josephson Junction]] 초전도 큐비트의 비선형성을 제공하는 조셉슨 접합 소자

## 공통 한계와 과제
- [[Quantum Decoherence]] 환경 결합으로 결맞음을 잃는 모든 하드웨어의 핵심 장벽

## 관련 MOC
- [[MOC - Quantum Error Correction]] 하드웨어 결함을 부호로 보호하는 층
- [[MOC - Quantum Computing]] 하드웨어 위에서 동작하는 게이트와 회로 계산 모형 층
- [[MOC - Foundations of Quantum Information]] 큐비트와 측정 등 토대 형식체계 층
