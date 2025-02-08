---
layout: page
title: TorchRL Modules
date: 2024-12-21 15:00:00+0900
description: Pytorch TorchRL Modules
img: assets/img/1.jpg
importance: 1
category: Torch RL
related_publications: false
toc:
  sidebar: left
---

### 1. TorchRL Modules 소개
- TorchRL에서 바라보는 강화학습(Reinforcement Learning)
	- 특정 과제를 효과적으로 해결할 수 있는 `정책(policy)`을 설계하는 것이 목표
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
    - 아래 예시만으로도 `정책(policy)`을 실행하는 데 필요한 모든 준비가 완료.
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
- 사용 예시
    - 아래 예시는 `정규 분포(Normal Distribution)`를 사용하는 `액터(actor)`를 구축하는 과정
        - 즉, 확률적 정책 설계의 기본 원리를 보여줌
    - MLP 백본(backbone)
	    - 크기가 3인 관측값을 입력받아 크기가 2인 텐서 출력
	- NormalParamExtractor 모듈
	    - MLP의 출력을 두 개의 부분, 평균(mean)과 표준편차(standard deviation), 으로 나눔
	    - 각각 크기는 1
        - 이 값은 `loc`와 `scale` 항목으로 반환
	- `ProbabilisticActor`
	    - 평균과 표준편차를 `in_keys`로 받아 분포를 생성.
	    - 샘플 및 로그 확률(log-probability)을 계산하고 `TensorDict`에 저장

```python
from tensordict.nn.distributions import NormalParamExtractor
from torch.distributions import Normal
from torchrl.modules import ProbabilisticActor

backbone = MLP(in_features=3, out_features=2)
extractor = NormalParamExtractor()
module = torch.nn.Sequential(backbone, extractor)
td_module = TensorDictModule(
    module, 
    in_keys=["observation"],
    out_keys=["loc", "scale"]
)
policy = ProbabilisticActor(
    td_module,
    in_keys=["loc", "scale"],
    out_keys=["action"],
    distribution_class=Normal,
    return_log_prob=True,
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
        loc: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
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
        sample_log_prob: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
        scale: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
        terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
    },
    batch_size=torch.Size([10]),
    device=None,
    is_shared=False
)
```

- set_exploration_type() 함수
    - `행동(action)`의 샘플링 방식을 제어하여, 랜덤 샘플 대신 분포의 기댓값(expected value)이나 다른 특성을 사용 가능

```python
from torchrl.envs.utils import ExplorationType, set_exploration_type

with set_exploration_type(ExplorationType.DETERMINISTIC):
    # takes the mean as action
    rollout = env.rollout(max_steps=10, policy=policy)

with set_exploration_type(ExplorationType.RANDOM):
    # Samples actions according to the dist
    rollout = env.rollout(max_steps=10, policy=policy)
```

### 6. 탐험 (Exploration)
- TorchRL은 **탐험 모듈(exploration modules)** 제공
    - EGreedyModule
    - AdditiveGaussianModule
    - OrnsteinUhlenbeckProcessModule
- 사용 예시
    - 탐험 모듈이 어떻게 작동하는지 보기 위해, 다시 결정론적 정책(deterministic policy)으로 전환
    - $$\epsilon$$-greedy 탐험 모듈
        - annealing frames(탐험 감소 프레임 수)와 $$\epsilon$$ 초기값으로 커스터마이즈
        - $$\epsilon=1$$인 경우: 모든 행동이 랜덤으로 선택
	    - $$\epsilon=0$$인 경우: 탐험 없이 완전히 결정론적 행동만 수행
	    - 탐험 요소($$\epsilon$$ 값)를 점진적으로 감소시키기 위해 `step()` 호출 필요
    - TensorDictSequential 모듈 사용
        - 탐험적 정책(explorative policy)을 구축하기 위해, 결정론적 정책 모듈과 탐험 모듈을 연결
        - `TensorDictSequential`은 `TensorDict` 환경에서의 `Sequential`과 유사한 역할을 수행
        
```python
from tensordict.nn import TensorDictSequential
from torchrl.modules import EGreedyModule

policy = Actor(MLP(3, 1, num_cells=[32, 64]))
exploration_module = EGreedyModule(
    spec=env.action_spec, annealing_num_steps=1000, eps_init=0.5
)

exploration_policy = TensorDictSequential(policy, exploration_module)

with set_exploration_type(ExplorationType.DETERMINISTIC):
    # Turns off exploration
    rollout = env.rollout(max_steps=10, policy=exploration_policy)

with set_exploration_type(ExplorationType.RANDOM):
    # Turns on exploration
    rollout = env.rollout(max_steps=10, policy=exploration_policy)
```

### 7. `Q-Value Actors`
- 일부 강화 학습 알고리즘에서는 `정책(policy)`이 독립된 모듈이 아니라, 다른 모듈을 사용하여 간접적으로 구축됨
- `Q-Value actors`가 이에 해당
- `Q-Value actors`는 `행동(action)` 값에 대한 추정(대부분의 경우 이산(discrete) 값)을 요구하며, 가장 높은 값을 가진 행동을 탐욕적으로 선택
- 특정 설정(유한 이산 행동 공간 및 유한 이산 상태 공간)에서는 상태-행동(state-action) 쌍을 2D 테이블에 저장
- **DQN(Deep Q-Network)**의 혁신은 신경망을 활용해 연속 상태 공간(continuous state spaces)에서도 Q(s, a) 값 맵을 인코딩할 수 있도록 확장
- 사용 예시
    - 이산 행동 공간을 가진 환경만 활용 가능
    - value_net: 환경으로 부터 상태를 받으면 각 개별 행동당 Q-value를 산출
    - Q-Value actor: `QValueModule` 활용
    - 해당 `actor`을 적용한 rollout 결과에 다음 두 개의 값에 주목
        - `action_value`
        - `chosen_action_value`

```python
env = GymEnv("CartPole-v1")
print(env.action_spec)
```
- 실행 결과

```plaintext
OneHot(
    shape=torch.Size([2]),
    space=CategoricalBox(n=2),
    device=cpu,
    dtype=torch.int64,
    domain=discrete
)
```

- 사용 예시

```python
from torchrl.modules import QValueModule

num_actions = 2
value_net = TensorDictModule(
    MLP(out_features=num_actions, num_cells=[32, 32]),
    in_keys=["observation"],
    out_keys=["action_value"],
)
policy = TensorDictSequential(
    value_net,  # writes action values in our tensordict
    QValueModule(spec=env.action_spec),  # Reads the "action_value" entry by default
)
rollout = env.rollout(max_steps=3, policy=policy)
print(rollout)
```
- 실행 결과

```plaintext
TensorDict(
    fields={
        action: Tensor(shape=torch.Size([3, 2]), device=cpu, dtype=torch.int64, is_shared=False),
        action_value: Tensor(shape=torch.Size([3, 2]), device=cpu, dtype=torch.float32, is_shared=False),
        chosen_action_value: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.float32, is_shared=False),
        done: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        next: TensorDict(
            fields={
                done: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
                observation: Tensor(shape=torch.Size([3, 4]), device=cpu, dtype=torch.float32, is_shared=False),
                reward: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.float32, is_shared=False),
                terminated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
                truncated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False)
            },
            batch_size=torch.Size([3]),
            device=None,
            is_shared=False
        ),
        observation: Tensor(shape=torch.Size([3, 4]), device=cpu, dtype=torch.float32, is_shared=False),
        terminated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        truncated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False)},
    batch_size=torch.Size([3]),
    device=None,
    is_shared=False
)
```
- 탐험 적용 사용 예시
    - `EGreedyModule` 사용

```python
from torchrl.modules import QValueModule, EGreedyModule

num_actions = 2
value_net = TensorDictModule(
    MLP(out_features=num_actions, num_cells=[32, 32]),
    in_keys=["observation"],
    out_keys=["action_value"],
)
policy_explore = TensorDictSequential(
    value_net,  # writes action values in our tensordict
    QValueModule(spec=env.action_spec),  # Reads the "action_value" entry by default
    EGreedyModule(spec=env.action_spec),
)
with set_exploration_type(ExplorationType.RANDOM):
    rollout_explore = env.rollout(max_steps=3, policy=policy_explore)
print(rollout)
```
- 실행 결과

```plaintext
TensorDict(
    fields={
        action: Tensor(shape=torch.Size([3, 2]), device=cpu, dtype=torch.int64, is_shared=False),
        action_value: Tensor(shape=torch.Size([3, 2]), device=cpu, dtype=torch.float32, is_shared=False),
        chosen_action_value: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.float32, is_shared=False),
        done: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        next: TensorDict(
            fields={
                done: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
                observation: Tensor(shape=torch.Size([3, 4]), device=cpu, dtype=torch.float32, is_shared=False),
                reward: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.float32, is_shared=False),
                terminated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
                truncated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False)
            },
            batch_size=torch.Size([3]),
            device=None,
            is_shared=False
        ),
        observation: Tensor(shape=torch.Size([3, 4]), device=cpu, dtype=torch.float32, is_shared=False),
        terminated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False),
        truncated: Tensor(shape=torch.Size([3, 1]), device=cpu, dtype=torch.bool, is_shared=False)
    },
    batch_size=torch.Size([3]),
    device=None,
    is_shared=False
)
```