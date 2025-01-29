---
layout: post
title: "Imitation Learning (모방 학습)"
description: "Imitation Learning (모방 학습)"
date: 2025-1-29 00:00:00
tags: Imitation Learning, Reinforcement Learning
categories: RL
tabs: true
related_publications: false
toc:
  sidebar: left
---

### **📌 0. Imitation Learning 종류**
- Behavioral Cloning
- Inverse Reinforcement Learning
- Adversarial Imitation Learning
- Imitation from Observation)**  

---

### **📌 1. Behavioral Cloning (BC)**  
BC(행동 복제)는 **감독 학습(Supervised Learning) 기반의 모방 학습(IL) 방법**으로, 전문가의 시연을 직접 학습하여 **상태(State) → 행동(Action) 매핑을 학습**하는 방식.  

#### **1.1 BC의 개념 및 특징**  
- **전문가의 시연 데이터(상태-행동 쌍)를 직접 학습**하여 정책을 구성.  
- **환경과의 추가적인 상호작용 없이 전문가의 정책을 복제할 수 있음**.  
- **컴퓨팅 효율성이 높고, 학습이 빠름**.  
- **환경의 동역학(Dynamics)을 고려하지 않음**.  

✅ **장점**: 빠르고 효율적인 학습 가능.  
⚠ **단점**: 공변량 이동(Covariate Shift) 문제로 인해 일반화 성능이 저하될 수 있음.  

---

#### **1.2 BC의 주요 한계: 공변량 쉬프트(Covariate Shift) 문제**  
- **훈련 중에는 전문가 데이터에서 학습하지만, 테스트 시에는 에이전트가 새로운 상태를 생성하는 문제 발생**.  
- 잘못된 행동을 수행하면 **전문가의 시연 데이터에서 벗어나며, 복구 방법을 학습하지 못함**.  

**➡ 해결 방법**:  
1. **대화형 모방 학습(Interactive IL)** → 전문가 개입을 활용하여 실시간으로 정책 보정 (예: DAgger, SafeDAgger).  
2. **전문가 정책 분포 추정 기반 보상 학습** → 전문가 정책을 따르도록 보상 함수 설계 후 RL로 최적화 (예: SQIL).  
3. **정책을 제한하여 전문가 상태 공간에서만 작동하도록 학습** → 전문가 궤적에서 벗어나지 않도록 제한 (예: 자율주행 연구).  

---

#### **1.3 BC의 추가적인 문제: 인과 관계 오인(Causal Misidentification) 문제**  
- BC는 전문가 행동을 단순 복제하는 방식이므로, **행동의 원인을 제대로 학습하지 못하는 경우 발생**.  
- 특히, **Copycat Problem**이 발생할 수 있음 → 에이전트가 전문가의 이전 행동을 단순히 따라 하는 현상.  

**➡ 해결 방법**:  
1. **Residual Action Prediction** → 과거 행동을 직접 예측하지 않고, "예측 보정(residual prediction)" 수행.  
2. **Memory Extraction Module** → 과거 행동 정보를 제거하고 현재 상태만을 이용한 행동 예측 수행.  

✅ **BC의 주요 연구 방향**: 전문가 개입을 최소화하면서도 일반화 성능을 높이는 방법 개발.  

---

### **📌 2. Inverse Reinforcement Learning (IRL)**  
IRL(역강화학습)은 **전문가의 행동에서 보상 함수를 추론하고, 이를 기반으로 정책을 학습하는 방식**.  

#### **2.1 IRL의 개념 및 특징**  
- **전문가가 최적 정책을 따른다고 가정하고, 보상 함수(Reward Function)를 추정**.  
- **추정된 보상 함수를 RL을 통해 최적화하여 최적 정책 학습**.  
- **환경과의 상호작용을 필요로 하며, 샘플 효율성이 낮음**.  

✅ **장점**: 환경의 동역학을 활용하여 더 일반화된 정책 학습 가능.  
⚠ **단점**: 보상 함수 학습 과정이 계산적으로 비효율적이며, 안전 문제가 발생할 수 있음.  

---

#### **2.2 IRL의 주요 한계 및 해결 방법**  
1. **샘플 효율성 문제 → 변분 추론(Variational Inference) 활용하여 개선 (예: AVRIL)**  
2. **보상 함수와 정책 간의 모호성 해결 → 최대 마진 기법(Maximum Margin Methods), 최대 엔트로피 IRL(MaxEntIRL) 적용**  

✅ **IRL의 주요 연구 방향**: 샘플 효율성을 개선하고, 보상 함수 추정의 불확실성을 줄이는 연구 진행.  

---

### **📌 3. Adversarial Imitation Learning (AIL)**  
AIL(적대적 모방 학습)은 **IRL의 계산 복잡성을 해결하기 위해 적대적 학습(Adversarial Training)을 적용한 방식**.  

#### **3.1 AIL의 개념 및 특징**  
- **GAN과 유사한 구조의 2인 게임(에이전트 vs. 판별기) 활용**.  
- **판별기(Discriminator)는 전문가와 에이전트의 상태-행동 분포 차이를 학습하고, 이를 보상으로 변환**.  
- **강화 학습(RL)을 통해 에이전트가 전문가의 행동을 따라가도록 학습**.  

✅ **장점**: IRL보다 더 빠르고 샘플 효율성이 높음.  
⚠ **단점**: 학습이 불안정할 수 있으며, 최적 정책이 수렴하지 않을 가능성이 있음.  

---

#### **3.2 AIL의 대표적인 방법들**  
1. **GAIL (Generative Adversarial Imitation Learning)** → 판별기의 혼란도를 보상으로 활용하여 전문가 정책을 복제.  
2. **Wasserstein AIL (PWIL)** → Wasserstein 거리를 활용하여 학습 안정성 개선.  

✅ **AIL의 주요 연구 방향**: 학습 안정성을 높이고, GAN 기반 최적화의 문제를 해결하는 방법 연구.  

---

### **📌 4. Imitation from Observation (IfO)**  
IfO(관찰을 통한 모방 학습)는 **행동 정보 없이 상태 정보만을 사용하여 학습하는 기법**.  

#### **4.1 IfO의 개념 및 특징**  
- **전문가의 행동(Action) 정보 없이, 상태(State) 정보만을 이용하여 모방 학습 수행**.  
- **인터넷 비디오(YouTube)와 같은 대규모 비정형 데이터 활용 가능**.  

✅ **장점**: 기존 IL보다 데이터 수집이 용이하며, 보다 자연스러운 모방 학습 가능.  
⚠ **단점**: 행동을 직접 관찰할 수 없으므로, 행동을 복원하는 과정에서 오류가 발생할 수 있음.  

---

#### **4.2 IfO의 대표적인 방법들**  
1. **BCO (Behavior Cloning from Observation)** → 행동을 역추론하는 방식으로 학습.  
2. **GAIfO (Generative Adversarial Imitation from Observation)** → GAIL 구조를 IfO에 적용하여 학습 안정성 개선.  
3. **Time-Contrastive Networks (TCN)** → 카메라 시점 차이를 극복하는 학습 기법.  

✅ **IfO의 주요 연구 방향**: 행동을 직접 관찰할 필요 없이, 상태 기반 보상 함수를 정밀하게 설계하는 연구 진행.  

---

### **📌 종합 결론**
- **BC → IRL → AIL → IfO로 연구가 발전하면서, 샘플 효율성과 일반화 성능이 개선되고 있음**.  
- **AIL과 IfO는 기존 IL의 한계를 극복하는 중요한 연구 방향이며, 향후 연구에서는 도메인 간 전이 학습과 행동 복원 성능 개선이 핵심 과제가 될 것**.  