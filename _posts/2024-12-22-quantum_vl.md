---
layout: post
title: Quantum Virtual Link Generation via Reinforcement Learning
description: Quantum Virtual Link Generation via Reinforcement Learning
tags: Quantum, Reinforcement Learning
categories: Quantum Networks
tabs: true
related_publications: true
toc:
  sidebar: left
---

### Abstract

- Quantum networks leverage quantum entanglement as a fundamental building block. 
- When two qubits are entangled, their states exhibit non-classical correlations, enabling novel applications such as quantum key distribution and distributed quantum computing, which are not possible with classical communication. 
- However, <u>quantum entanglement is a probabilistic process heavily dependent on the characteristics of the involved devices</u>, such as optical fibers, lasers, and quantum memories. 
- Managing this process to maintain entanglement with high quality for as long as possible is a <strong>stochastic control problem</strong>.
- This process can be modeled as a MDP and solved using the RL framework. 
- In this work, we employ RL to develop an <strong>entanglement management policy</strong> that surpasses the current State-of-the-Art policies, particularly in scenarios where precise models of the quantum devices are unavailable.
- Reference: {% cite a10207249 %}


### Introduction

## 양자 네트워크와 양자 얽힘 관리: 주요 내용 정리

### 1. **양자 인터넷의 등장**
- 최근 **양자 물리학 원리(quantum physics principles)**가 컴퓨터 네트워크에 적용되며 연구와 산업 분야에서 주목받고 있음.
- **IETF(Internet Engineering Task Force)**가 제안한 **양자 인터넷(Quantum Internet)**의 표준화 시도가 이를 증명.
- **양자 얽힘(quantum entanglement)**은 양자 통신(Quantum Communication)을 위한 기본 자원으로 확인됨.
  - 이를 통해 **양자 암호 키 분배(quantum key distribution)**와 **분산 양자 컴퓨팅(distributed quantum computing)**과 같은 응용 가능.

---

### 2. **양자 얽힘의 특성과 문제**
- 양자 얽힘은 **확률적 과정(probabilistic process)**으로, 관련 통신 장치(광섬유(optical fiber), 레이저(laser), 양자 메모리(quantum memory) 등)의 특성에 크게 의존.
- 얽힘 관리는 **확률 제어 문제(stochastic control problem)**로, **마르코프 결정 과정(Markov Decision Process, MDP)**으로 공식화 가능.
- 본 연구에서는 두 원격 통신 노드(remote communication nodes) 간의 얽힘을 설정할 때 **심층 강화 학습(Deep Reinforcement Learning, DRL)**의 적용 가능성을 조사.

---

### 3. **양자 비트(Qubit)와 얽힘(Entanglement)**
- **양자 비트(Qubit)**는 고전 비트(classical bit)의 양자적 대응물.
  - 고전 비트는 “0” 또는 “1”의 상태만 가지지만, 양자 비트는 두 상태의 **중첩(superposition)** 상태를 가짐.
  - 측정 후 확률에 따라 “0” 또는 “1” 상태로 결정됨.
- **얽힘(Entanglement)**:
  - 두 양자 비트가 얽히면 각 상태를 독립적으로 설명할 수 없음.
  - 한 쪽 비트 상태가 변하면, 물리적 거리에 관계없이 다른 비트의 상태도 함께 변화.
  - 얽힘은 양자 암호와 분산 양자 컴퓨팅 같은 비고전적(non-classical) 응용의 핵심 요소.

---

### 4. **양자 네트워크(Quantum Network)**
- 양자 네트워크는 얽힘 상태를 분배하고 **양자 비트(Qubit)**를 교환할 수 있는 노드(node)들로 구성.
  - 노드들은 **광섬유(optical fiber)** 또는 **위성 레이저 링크(satellite laser link)**로 연결.
- 얽힘 상태 설정:
  - 두 인접 노드에 위치한 양자 비트 간 얽힘은 **기본 양자 링크(elementary quantum link)**를 구성.
  - 성공 확률(**Pe**)은 거리 증가에 따라 지수적으로 감소.
- **가상 링크(Virtual Link)** 생성:
  - 얽힘 교환(Entanglement Swapping)을 통해 두 기본 링크를 결합.
  - 예) 기본 링크 A-B와 B-C를 소비하여 A-C라는 장거리 가상 링크 생성.
  - 얽힘 교환을 수행하는 중간 노드는 **양자 리피터(Quantum Repeater)**로, 중간 링크를 **양자 메모리(Quantum Memory)**에 저장 후 사용.

---

### 5. **양자 메모리 수명(Quantum Memory Lifetimes)**
- 양자 메모리에 저장된 얽힘 상태가 원래 상태를 유지할 확률(**메모리 효율(memory efficiency), ηm**)은 시간이 지남에 따라 감소.
  - 이 과정은 **데코히어런스(Decoherence)**로 알려짐.
  - 얽힘 교환 성공 확률(**Ps**)은 가장 오래된 양자 메모리의 메모리 효율 ηm에 의존.

---

### 6. **본 연구의 기여**
- 양자 가상 링크 생성 과정을 **고전적 MDP(Classical MDP)**로 모델링하고, DRL 알고리즘을 사용하여 최적의 생성 정책(policy)을 도출.
- 본 연구는 기본 링크의 **나이(age)**를 추적하는 새로운 방법을 제안:
  - 기존 연구에서는 링크 생성 성공 시점의 타임스탬프를 고려하지 않음.

---

### 7. **논문 구조**
- **Section 2**: 관련 연구(related works) 소개.
- **Section 3**: 가상 링크 생성의 MDP 모델링 및 DRL 접근법 설명.
- **Section 4**: 수치 결과(numerical results) 및 실험 설정.
- **Section 5**: 결론(conclusion) 제시.


{% include figure.liquid loading="eager" path="assets/img/blogs/2024-12-22-quantum_vl/link.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
