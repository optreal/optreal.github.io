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
- `TensorDict`를 입력과 출력으로 사용하는 PyTorch 모듈을 정의하는 클래스.
    - 표준 모듈(Module)이나 함수를 `TensorDictModules`로 감싸서, 필요한 항목을 읽고 모듈에 전달한 뒤 결과를 정해진 항목에 기록
- 환경이 `TensorDict`와 상호작용하는 방식처럼, 정책(`policy`)과 가치 함수(`value function`)를 나타내는 모듈도 동일하게 `TensorDict`와 상호작용할 수 있도록 만들기 위해 사용하는 모듈
- 사용 예시 
    - 예제 정책: 결정적 맵(deterministic map)
        - 관측 공간에서 행동 공간으로의 결정론적 맵을 사용한 가장 간단한 정책
        - 범용성을 위해 `LazyLinear` 모듈을 활용
    - 아래 예시만으로도 정책(policy)을 실행하는 데 필요한 모든 준비가 완료.
    - Lazy 모듈을 사용하면 관측 공간(observation space)의 크기를 직접 가져올 필요가 없으며, 모듈이 이를 자동으로 결정
    - 이제 이 정책은 환경과 상호작용할 준비가 됨

```python
import torch

from tensordict.nn import TensorDictModule
from torchrl.envs import GymEnv

env = GymEnv("Pendulum-v1")
module = torch.nn.LazyLinear(out_features=env.action_spec.shape[-1])
policy = TensorDictModule(
    module,
    in_keys=["observation"],
    out_keys=["action"],
)

rollout = env.rollout(max_steps=10, policy=policy)
print(rollout)
```

- 실행 결과

```plaintext
TensorDict(
    fields={
        action: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
        done: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        next: TensorDict(
            fields={
                done: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
                observation: Tensor(shape=torch.Size([10, 3]), device=cpu, dtype=torch.float32, is_shared=False),
                reward: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
                terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
                truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
            },
            batch_size=torch.Size([10]),
            device=None,
            is_shared=False
        ),
        observation: Tensor(shape=torch.Size([10, 3]), device=cpu, dtype=torch.float32, is_shared=False),
        terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
    },
    batch_size=torch.Size([10]),
    device=None,
    is_shared=False
)
```

### 3. 특별한 `TensorDictModules`
- 특별한 `TensorDictModules`
    - `Actor`
        - `Actor`는 `in_keys`와 `out_keys`에 대한 기본값을 제공하여, 많은 일반적인 환경과 간단히 통합 가능 
    - `ProbabilisticActor`
    - `ActorValueOperator`
    - `ActorCriticOperator`
- 사용 예시

```python
import torch

from tensordict.nn import TensorDictModule
from torchrl.envs import GymEnv

env = GymEnv("Pendulum-v1")
module = torch.nn.LazyLinear(out_features=env.action_spec.shape[-1])
policy = Actor(module)
rollout = env.rollout(max_steps=10, policy=policy)
```

### 4. 네트워크
- TorchRL은 `TensorDict` 기능을 사용하지 않고도 사용할 수 있는 일반적인 모듈을 제공
- 가장 흔히 사용되는 네트워크는 `MLP (Multi-Layer Perceptron)`와 `ConvNet (CNN)` 모듈
- 사용 예시

```python
from torchrl.modules import MLP

module = MLP(
    out_features=env.action_spec.shape[-1],
    num_cells=[32, 64],
    activation_class=torch.nn.Tanh,
)
policy = Actor(module)
rollout = env.rollout(max_steps=10, policy=policy)
```

### 5. 확률적 정책 (Probabilistic policies)
- `PPO(Policy-Optimization Algorithms)`와 같은 정책 최적화 알고리즘은 확률적 정책을 사용
- 이 때, 정책 모듈은 관측 공간에서 가능한 행동에 대한 분포를 나타내는 파라미터 공간으로의 매핑이 필요
- TorchRL은 다음과 같은 작업들을 하나의 클래스에서 처리하여 이러한 모듈 설계를 간소화함
    - 파라미터에서 분포 생성
	- 해당 분포에서 샘플링
	- 로그 확률(log-probability) 계산 및 반환