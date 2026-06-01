---
title: MOC - Quantum Information Theory
aliases: [양자정보이론, Quantum Information Theory, QIT]
type: moc
status: budding
domain: quantum-information-theory
tags:
  - concept/entanglement
  - qkd/e91
  - concept/formalism
created: 2026-05-30
updated: 2026-05-30
up: "[[MOC - Foundations of Quantum Information]]"
---

# MOC - Quantum Information Theory

## 개요
양자정보이론은 양자 상태에 담긴 정보를 자원으로 보고, 그 자원을 어떻게 표현하고 변환하고 전송하는지를 다룬다. 핵심 자원은 얽힘이다. 얽힘은 고전 통신만으로는 만들 수 없는 상관을 두 주체 사이에 마련해 두고, 이를 소비하면서 텔레포테이션이나 초고밀도 부호화 같은 고전적으로 불가능한 작업을 가능하게 한다. 이 지도는 얽힘 자원을 중심으로 정보 처리 프로토콜과 그 보안 응용으로 가는 진입 경로를 모은다.

이 MOC는 양자정보이론 도메인의 진입점으로서 직접 설명을 길게 두지 않고 개별 개념 노트로 가는 링크 허브 역할만 한다. 얽힘의 형식적 토대에서 출발해 얽힘을 소비하는 통신 프로토콜로, 다시 거리 한계를 넘기는 양자 중계와 측정 기반 보안 프로토콜로 진입 경로를 펼친다.

## 핵심 개념
- [[Quantum Entanglement]] 부분계로 분리되지 않는 상관이자 정보이론의 핵심 자원
- [[Bell States]] 최대 얽힘 두 큐비트 상태이자 프로토콜의 기본 단위
- [[Density Matrix]] 혼합 상태와 부분계 기술의 밀도 연산자 형식
- [[Partial Trace]] 복합계에서 부분계 상태를 얻는 축소 연산
- [[Quantum Measurement]] 상태 붕괴와 정보 추출의 측정 공준
- [[No-Cloning Theorem]] 임의 미지 상태 복제 불가라는 근본 한계

## 얽힘 자원과 응용
- [[Quantum Teleportation]] 얽힘과 고전 통신으로 미지 상태를 옮기는 프로토콜
- [[Superdense Coding]] 얽힘을 소비해 한 큐비트로 두 고전 비트를 전송하는 부호화

## QKD 연계와 장거리 양자 통신
- [[Device-Independent Cryptography]] 측정 통계만으로 보안을 증명하는 장치 독립 암호
- [[Quantum Repeater]] 얽힘 교환과 정제로 거리 한계를 넘기는 양자 중계기
- [[Cascade Protocol]] 키 합의 후 오류를 대화형으로 정정하는 정보 조정 프로토콜

## 관련 MOC
- [[MOC - Foundations of Quantum Information]] 상태와 측정 공준 등 형식적 토대 층
- [[MOC - Quantum Computing]] 게이트와 회로 등 계산 모형 층
- [[MOC - Quantum Cryptography]] 측정 교란과 복제 불가를 보안 근거로 쓰는 양자암호 층
- [[MOC - Post-Quantum Cryptography]] 고전 비대칭 암호의 양자 내성 대체 층
