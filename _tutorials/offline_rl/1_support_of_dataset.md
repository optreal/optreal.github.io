---
layout: page
title: Support of The Dataset
date: 2025-02-07 15:00:00+0900
description: Offline Reinforcement Learning
img: assets/img/1.jpg
importance: 1
category: Offline RL
related_publications: false
toc:
  sidebar: left
---

### 🔹 **Support의 의미**
- 확률 분포에서의 "지지 집합 (support)"  
  - 임의의 확률 변수 $$x$$에 대하여 임의의 확률 분포 $$P(x)$$가 있다고 할 때, $$P(x) > 0$$인 모든 $$x$$들의 집합을 지지 집합(support set)이라고 한다.  
  - 즉, 확률적으로 나타날 가능성이 있는 값들의 범위를 의미한다.

- 데이터셋에서의 "지지(support) 공간"
  - RL에서는 데이터셋이 포함하는 (상태, 행동)의 $(s, a)$ 쌍 분포를 고려할 때 데이터셋에 존재하는 그러한 쌍들이 $$P(a \vert s) > 0$$을 만족하는 것들의 집합을 "support of the dataset"라고 정의한다.
  - 반대로, 데이터셋에서 관측된 적 없는 쌍들은 $$P(a \vert s) = 0$$이므로, 데이터의 지지 집합 **바깥(out-of-support)**에 존재하는 쌍들이 된다.

### 🔹 **문장 속 의미**
- *"To avoid extrapolation error, we need to constrain the policy to select actions within the support of the dataset."*  
  - 즉, **"추론 오류(Extrapolation error)를 방지하기 위해, 정책이 데이터셋에 포함된 행동의 분포 내에서만 행동을 선택하도록 제한해야 한다."**  
- **정책(policy)이 학습 데이터셋에서 관찰되지 않은 행동을 선택하지 않도록 제약해야 한다**는 의미다.

### 🔹 **실제 적용 예시**
- 오프라인 강화학습에서는 정책이 **데이터셋에 존재하지 않는 행동을 선택하면 신뢰할 수 없는 Q-value가 계산**될 수 있다.  
- 이를 방지하기 위해, 다음과 같은 방법을 사용하여 정책을 데이터셋의 지지 집합 안에 유지한다:
  - 1) **Behavior Cloning (BC, 행동 복제)**  
    - 정책이 행동을 데이터셋에서 추출된 행동과 가깝도록 유도.  
  - 2) **KL-divergence 또는 MMD(Maximum Mean Discrepancy) 기반 정규화**  
    - 정책이 데이터셋과 다른 행동을 선택하려 할 때 패널티를 부과.  
  - 3) **Latent Action Space 활용 (예: PLAS 기법)**  
    - 행동을 직접 모델링하지 않고, 데이터셋 내의 행동 분포를 유지하도록 잠재 공간에서 샘플링.
- 즉, *"support of the dataset"*은 **정책이 선택할 수 있는 "안전한 행동 집합"**이라고 볼 수 있다.
