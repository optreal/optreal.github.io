---
layout: page
title: TorchRL Modules
date: 2024-12-21 15:00:00+0900
description: Pytorch TorchRL Modules
img: assets/img/1.jpg
importance: 1
category: TorchRL
related_publications: false
toc:
  sidebar: left
---

### 1. TorchRL Modules 소개
- TorchRL에서 바라보는 강화학습(Reinforcement Learning)
	- 특정 과제를 효과적으로 해결할 수 있는 정책(`policy`)을 설계하는 것이 목표
	- 정책은 다양한 형태를 가질 수 있음:
	    - 관측 공간에서 행동 공간으로의 미분 가능한 매핑
	    - 가능한 행동에 대해 계산된 `Value` 리스트에서 `argmax`를 선택하는 방식 등
- TorchRL에서 바라보는 정책의 특징
	-	결정적(deterministic) 또는 확률적(stochastic)
	-	RNN(Recurrent Neural Network), Transformer 등 복잡한 요소를 포함할 수 있음
    - 	복잡한 시나리오 대응
	    -	다양한 정책 유형을 수용하는 것은 복잡할 수 있음
	    -	TorchRL은 이러한 정책 설계를 간단히 처리할 수 있는 기능을 제공
- 이 튜토리얼의 초점
	- 정책 설계의 핵심 기능을 간략히 설명
	- 확률적(stochastic) 및 Q-Value 정책에 초점
	- MLP 및 CNN을 백본(backbone)으로 사용하는 두 가지 일반적인 시나리오 소개

### 2. `TensorDictModules`
- 환경이 `TensorDict`와 상호작용하는 방식처럼, 정책(`policy`)과 가치 함수(`value function`)를 나타내는 모듈도 동일하게 `TensorDict`와 상호작용
- 표준 모듈(Module)이나 함수를 클래스로 감싸서, 필요한 항목을 읽고 모듈에 전달한 뒤 결과를 정해진 항목에 기록하는 것
- 예제 정책: 결정적 맵(deterministic map)
    - 관측 공간에서 행동 공간으로의 결정론적 맵을 사용한 가장 간단한 정책
	- 범용성을 위해 `LazyLinear` 모듈을 활용