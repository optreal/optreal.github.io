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

---

### Introduction

#### 1. 양자 인터넷의 등장
- 최근 **양자 물리학 원리(quantum physics principles)**가 컴퓨터 네트워크에 적용되며 연구와 산업 분야에서 주목받고 있음
- **IETF(Internet Engineering Task Force)**가 제안한 **양자 인터넷(Quantum Internet)**의 표준화 시도가 이를 증명 {% cite rfc9340 %}, {% cite rfc9583 %}
- **양자 얽힘(quantum entanglement)**은 양자 통신(Quantum Communication)을 위한 기본 자원
  - 이를 통해 **양자 암호 키 분배(quantum key distribution)**와 **분산 양자 컴퓨팅(distributed quantum computing)**과 같은 응용 실현 가능

#### 2. 양자 얽힘의 특성과 문제
- 양자 얽힘은 **확률적 과정(probabilistic process)**으로, 관련 통신 장치(광섬유(optical fiber), 레이저(laser), 양자 메모리(quantum memory) 등)의 특성에 크게 의존
- 얽힘 관리는 **확률 제어 문제(stochastic control problem)**로, 마르코프 결정 과정(Markov Decision Process, MDP)**으로 공식화 가능
- 본 연구에서는 두 원격 통신 노드(remote communication nodes) 간의 얽힘을 설정할 때 DRL의 적용 가능성 조사

#### 3. **양자 비트(Qubit)와 얽힘(Entanglement)**
- **양자 비트(Qubit)**는 고전 비트(classical bit)의 양자적 대응물
  - 고전 비트는 “0” 또는 “1”의 상태만 가지지만, 양자 비트는 두 상태의 **중첩(superposition)** 상태를 가짐
  - 측정 후 확률에 따라 “0” 또는 “1” 상태로 결정됨
- **얽힘(Entanglement)**:
  - 두 양자 비트가 얽히면 각 상태를 독립적으로 설명할 수 없음
  - 한 쪽 비트 상태가 변하면, 물리적 거리에 관계없이 다른 비트의 상태도 함께 변화
  - 얽힘은 양자 암호와 분산 양자 컴퓨팅 같은 비고전적(non-classical) 응용의 핵심 요소

#### 4. **양자 네트워크(Quantum Network)**

{% include figure.liquid loading="eager" path="assets/img/posts/2024-12-22-quantum_vl/link.png" class="img-fluid rounded z-depth-1" zoomable=true %}

- 양자 네트워크는 얽힘 상태를 분배하고 **양자 비트(Qubit)**를 교환할 수 있는 노드(node)들로 구성.
  - 노드들은 **광섬유(optical fiber)** 또는 **위성 레이저 링크(satellite laser link)**로 연결.

- 얽힘 상태 설정:
  - 두 인접 노드에 위치한 양자 비트 간 얽힘은 **기본 링크(elementary link)**를 구성.
  - 두 노드 간 얽힘 성공 확률($$P_e$$)은 거리 증가에 따라 지수적으로 감소
    - Short-distance entanglements (like A-B, in Fig. 1) are more likely to succeed than long-distance entanglements (like A-C, in Fig. 1)

- **가상 링크(Virtual Link)** 생성:
  - **얽힘 교환(Entanglement Swapping)**을 통해 두 개의 기본 링크를 결합
    - 예) 기본 링크 A-B와 B-C를 소비하여 A-C라는 장거리 가상 링크 생성
  - 얽힘 교환을 수행하는 중간 노드는 **양자 리피터(Quantum Repeater)**
  - 양자 리피터는 기본 링크들을 **양자 메모리(Quantum Memory)**에 저장 후 사용
    - 예) B는 A-B와 B-C의 기본 링크를 양자 메모리에 저장 후 사용

#### 5. **양자 메모리 수명(Quantum Memory Lifetimes)**
- 양자 메모리에 저장된 얽힘 상태가 원래 상태를 유지할 확률(**메모리 효율(memory efficiency), $$\eta_m$$**)은 시간이 지남에 따라 감소
  - 이 과정은 **데코히어런스(Decoherence)**로 알려짐.
  - 얽힘 교환 성공 확률(**$$P_s$$**)은 가장 오래된 양자 메모리의 메모리 효율 $$\eta_m$$에 의존

#### 6. 본 연구의 기여
- 양자 가상 링크 생성 과정을 **고전적 MDP(Classical MDP)**로 모델링하고, DRL 알고리즘을 사용하여 최적의 생성 정책(policy)을 도출
- 본 연구는 기본 링크의 **나이(age)**를 추적하는 새로운 방법을 제안:
  - 기존 연구에서는 링크 생성 성공 시점의 타임스탬프, 즉 링크의 나이를 고려하지 않음


| **심볼** | **설명** | **비고** |
| $$P_e$$ | 두 노드 간 얽힘 성공 확률 |   |
| $$\eta_m$$ | 양자 메모리에 저장된 얽힘 상태가 원래 상태를 유지할 확률 |   |
| $$P_s$$ | 얽힘 교환 성공 확률 |  가장 오래된 양자 메모리의 메모리 효율, 즉, $$\eta_m$$에 의존 |
{:.mbtablestyle .table .table-striped}

---

### Related Works

#### 1. Quantum Decision Process(QDP)
- Quantum Decision Process(QDP)는 MDP의 양자적 일반화로, Khatri의 박사 논문{% cite 10.5555/AAI29111215 %}에서 제안됨.
- 양자 컴퓨터의 사용을 전제로 하는 QDP의 주요 구성 요소
  -양자 상태(Quantum State)
	  - QDP에서는 상태를 **양자 상태(Quantum State)**로 표현
	  - 양자 상태는 상태 벡터로 나타나며, 중첩(Superposition)과 얽힘(Entanglement)을 포함할 수 있음
	- 양자 행동(Quantum Action)
	  - QDP의 행동은 고전적 액션이 아닌 **양자 연산자(Quantum Operator)**로 표현됨
	  -	이는 유니터리 연산이나 측정과 같은 양자역학적 연산으로 구현되며, 상태에 작용하여 새로운 상태를 생성함
	- 양자 상태 전이
	  - 전이 확률이 고전적 확률 분포 대신 양자 연산에 의해 결정됨
	  - 양자 연산은 상태를 변환하면서 고전적 시스템과 달리 비선형적이고 중첩된 결과를 생성할 수 있음
	- 보상 함수(Reward Function)
	  - QDP의 보상 함수는 양자 상태를 기준으로 정의되며, 특정 양자 상태 또는 결과를 얻는 것에 대한 가치(value)를 표현함
	  - 보상 함수는 상태와 행동의 조합에 따라 달라질 수 있음

#### 2. 이 논문의 접근 방식
- 기존 QDP 모델과는 달리, 이 논문은 측정된 물리적 속성으로 기술된 상태와 거시적 수준의 액션으로 구성된 **클래식 MDP**로 모델링함.
- Khatri의 아이디어 중 현재 기술 수준에서 활용 가능한 것들을 도입, 양자 컴퓨터 개발을 기다릴 필요 없이 구현 가능성을 제시함.

#### 3. 논문 모델링 대상
- 논문의 대상은 Khatri 논문의 부록 D에서 제시된 **얽힘 교환(entanglement swapping)을 이용한 가상 링크 생성**
- 가상 링크 생성은 다음 두 가지 맥락에서 연구됨:
  - **양자 중계기(Quantum Repeaters) 체인**에서의 장거리 얽힘 생성
  - **양자 얽힘 라우팅(Quantum Entanglement Routing)**에서의 Mesh 네트워크 기반 얽힘 생성

#### 4. 기존 연구와의 차이점
- 기존 연구는 링크 생성의 **히스토리(타임스탬프)**를 무시
- 즉, 가상 링크 생성 시 무한 메모리 컷오프 시간 정책(infinity memory cutoff-time policy)을 따름
  - 초기 링크가 성공적으로 얽힌 이후 다음 링크가 성공적으로 얽힐 때까지 초기 링크는 원래 상태를 지속적으로 유지할 확률을 100% 로 설정
  - 오래된 링크의 더 큰 디코히어런스(decoherence)가 얽힘 교환 성공 확률($$P_s$$)에 미치는 영향을 고려하지 않음

#### 5. 이 논문의 개선점
- 기존 접근법의 단점을 보완하여, 히스토리와 디코히어런스 영향을 포함한 MDP 모델링을 제안.

---

### Reinforcement Learning for Virtual Link Generation

#### 1. 문제 정의 
- 얽힘 교환을 통해 두 개의 기본 링크로부터 가상 링크를 생성할 때 **<u>시간당 성공적인 얽힘 교환 횟수(가상 링크 생성률)를 최대화</u>**하는 문제
- 얽힘 교환 성공 확률(**$$P_s$$**):
  - 두 개의 기본 링크가 성공적으로 생성되어야 얽힘 교환 시도 가능
  - 처음 생성된 기본 링크가 오래될수록 성공 확률 감소
- **컷오프 시간(**$$t_c$$**)**
  - <u>얽힘 상태를 유지하다가 폐기하는 시점을 결정하는 시간 임계값</u>
  - 얽힘 품질 관리
    - 얽힘 상태는 시간이 지남에 따라 디코히어런스 현상으로 인해 점차 품질이 저하됨
    - 품질이 낮아지면 얽힘 교환의 성공 확률이 감소하므로, 특정 시간 이후 품질 저하된 얽힘 상태를 폐기하는 것이 유리함
  - 리소스 최적화
    - 얽힘 상태를 오래 유지하면 성공 확률은 감소하지만, 새로 생성하지 않으므로 리소스를 절약할 수 있음
    - 반대로, 얽힘 상태를 폐기하고 새로 생성하면 높은 성공 확률을 얻을 수 있지만, 생성 과정에서 비용과 시간이 소모되고 얽힘 교환 시도를 지연시킴
  - 얽힘 폐기와 재생성을 통해 얽힘 교환 성공 확률(**$$P_s$$**)을 높일 수 있음

- 컷오프 시간 설정의 중요성
  - 짧은 컷오프 시간
    - 얽힘 상태를 빠르게 폐기하고 새로 생성함
    - 얽힘 교환 성공 확률은 높아질 수 있지만, 새로운 링크 생성의 반복으로 인해 전체 과정이 지연될 가능성이 높아짐
    - 리소스 소모 증가
  - 긴 컷오프 시간
	  - 얽힘 상태를 오래 유지하며 새로 생성하는 빈도를 줄임
	  - 얽힘 교환의 성공 확률은 낮아질 수 있지만, 리소스를 절약하며 빠르게 교환 시도를 진행할 수 있음
	  - 디코히어런스 영향이 크다면 교환 실패 가능성이 높아짐
- 컷오프 시간 결정의 어려움
	- 얽힘 생성 성공 확률($$P_e$$)와 얽힘 교환 성공 확률($$P_s$$)은 다양한 환경적 요인(예: 광섬유 길이, 양자 메모리 특성)에 따라 변화
	- 최적의 컷오프 시간을 설정하려면 다음을 고려해야 함
	  - 얽힘 상태의 품질 변화 속도(디코히어런스 영향)
	  - 새로운 얽힘 생성의 성공 확률 및 소요 시간
	  - 전체 가상 링크 생성률을 극대화하는 시간-성능 균형
 