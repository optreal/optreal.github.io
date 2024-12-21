---
layout: page
title: TorchRL
date: 2024-12-21 15:00:00+0900
description: Pytorch TorchRL
img: assets/img/12.jpg
importance: 1
category: reinforcement learning
related_publications: false
toc:
  sidebar: left
---

### 1. TorchRL Environment
- TorchRL은 직접적으로 환경을 제공하지 않고, 대신 다른 환경 시뮬레이터 라이브러리들을 위한 wrapper 제공 
- `torchrl.envs` 모듈의 역할 
  - 일반적인 환경 API를 제공
  - 다음과 같은 simulation backend의 중앙 허브 역할 수행
    - **BraxEnv**, **BraxWrapper**
    - **DMControlEnv**, **DMControlWrapper**
    - **GymEnv**, **GymWrapper**
    - **HabitatEnv**,
    - **IsaacGymEnv**, **IsaacGymWrapper**
    - **JumanjiEnv**, **JumanjiWrapper**
    - **MeltingpotEnv**, **MeltingpotWrapper**
    - **MOGymEnv**, **MOGymWrapper**
    - **MultiThreadedEnv**, **MultiThreadedEnvWrapper**
    - **OpenMLEnv**,
    - **OpenSpielEnv**, **OpenSpielWrapper**
    - **PettingZooEnv**, **PettingZooWrapper**
    - **RoboHiveEnv**,
    - **set_gym_backend**,
    - **SMACv2Env**, **SMACv2Wrapper**
    - **UnityMLAgentsEnv**, **UnityMLAgentsWrapper**
    - **VmasEnv**, **VmasWrapper**
- TorchRL에서 GymWrapper와 GymEnv는 각각의 사용 용도가 조금 다르며, 둘 다 Gym 환경과의 상호작용을 위해 설계되었음

#### GymEnv
- `GymEnv`는 Gym 환경을 TorchRL에서 직접 사용할 수 있도록 encapsulation(포장)하는 역할 수행
- TorchRL의 기본적인 환경 API를 따르며, Gym의 환경을 그대로 활용할 수 있도록 함
- 주로 Gym 환경을 처음 생성하거나 로드할 때 사용
- 주요 특징:
    - TorchRL 표준화된 API를 통해 Gym 환경에 접근 가능
    - `reset`, `step`, `render`와 같은 메서드가 제공됨
- 사용 예시:

```python
from torchrl.envs.libs.gym import GymEnv

# Gym 환경을 TorchRL 환경으로 변환
env = GymEnv("CartPole-v1")
state = env.reset()
print(state)
# TensorDict(
#     fields={
#         done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         observation: Tensor(shape=torch.Size([4]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)},
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```

#### GymWrapper
- `GymWrapper`는 이미 생성된 GymEnv 또는 유사한 환경을 추가적으로 변환하거나 TorchRL 표준에 맞게 확장하기 위해 사용
- `GymWrapper`는 기존의 Gym 환경에 새로운 기능을 추가하거나, 입력/출력 포맷을 변경할 때 사용
- 주로 environment wrapping을 통해 관측값이나 동작 공간 등을 변환하거나, 추가적인 전처리/후처리를 적용할 때 사용
- 사용 예시:

```python
import gymnasium as gym
from torchrl.envs.libs.gym import GymWrapper

# Wrapper를 통해 관측값 변환 등 추가 작업 수행
base_env = gym.make("CartPole-v1")
env = GymWrapper(base_env)
state = env.reset()
td = env.rand_step()
print(td)
# TensorDict(
#     fields={
#         action: Tensor(shape=torch.Size([2]), device=cpu, dtype=torch.int64, is_shared=False),
#         next: TensorDict(
#             fields={
#                 done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 observation: Tensor(shape=torch.Size([4]), device=cpu, dtype=torch.float32, is_shared=False),
#                 reward: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.float32, is_shared=False),
#                 terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)
#             },
#             batch_size=torch.Size([]),
#             device=None,
#             is_shared=False
#         )
#     },
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```
```python
import gymnasium as gym
from torchrl.envs.libs.gym import GymWrapper

# Wrapper를 통해 관측값 변환 등 추가 작업 수행
base_env = gym.make("CartPole-v1")
env = GymWrapper(base_env)
print(env.available_envs)
# ['Acrobot-v1', 'Ant-v2', 'Ant-v3', 'Ant-v4', 
#  'BipedalWalker-v3', 'BipedalWalkerHardcore-v3', 
#  'Blackjack-v1', 
#  'CarRacing-v2', 
#  'CartPole-v0', 'CartPole-v1', 
#  'CliffWalking-v0', 
#  'FrozenLake-v1', 'FrozenLake8x8-v1', 
#  'GymV21Environment-v0', 
#  'GymV26Environment-v0', 
#  'HalfCheetah-v2', 'HalfCheetah-v3', 'HalfCheetah-v4', 
#  'Hopper-v2', 'Hopper-v3', 'Hopper-v4', 
#  'Humanoid-v2', 'Humanoid-v3', 'Humanoid-v4', 'HumanoidStandup-v2', 'HumanoidStandup-v4', 
#  'InvertedDoublePendulum-v2', 'InvertedDoublePendulum-v4', 'InvertedPendulum-v2', 'InvertedPendulum-v4', 
#  'LunarLander-v2', 'LunarLanderContinuous-v2', 
#  'MountainCar-v0', 'MountainCarContinuous-v0', 
#  'Pendulum-v1', 'Pusher-v2', 
#  'Pusher-v4', 
#  'Reacher-v2', 'Reacher-v4', 
#  'Swimmer-v2', 'Swimmer-v3', 'Swimmer-v4', 
#  'Taxi-v3', 
#  'Walker2d-v2', 'Walker2d-v3', 'Walker2d-v4', 
#  'phys2d/CartPole-v0', 'phys2d/CartPole-v1', 
#  'phys2d/Pendulum-v0', 
#  'tabular/Blackjack-v0', 
#  'tabular/CliffWalking-v0']
```

#### GymEnv vs. GymWrapper 차이점 요약

| **항목** | **GymEnv** | **GymWrapper** |
| **사용 목적** | **Gym** 환경을 **TorchRL**의 표준 API로 변환 | 이미 생성된 환경에 추가적인 기능을 부여하거나 변환  |
| **적용 대상** | 기존 **Gym** 환경 | **GymEnv** 또는 유사한 환경 |
| **사용 시점** | 환경 초기 생성 단계 | 환경 생성 후 추가 변환이나 전처리가 필요할 때 |
| **주요 작업** | **reset**, **step**, **render**와 같은 기본 작업 제공 | 관측값 변환, 동작 공간 변경, 환경 속성 추가 |
{:.mbtablestyle .table .table-striped}

### 2. TorchRL Environment 활용 기본

- TorchRL에서 Environment은 두 가지 중요한 메서드를 제공함
    - reset(): 에피소드를 초기화하는 메서드로, 새로운 시작 상태 반환
	- step(action): 배우(Actor)가 선택한 행동(Action)을 실행하는 메서드로, 다음 상태, 보상, 완료 여부 등의 결과 반환

- 이 두 메서드는 `TensorDict`라는 데이터 구조를 읽고 쓰는 방식으로 동작함
    - `TensorDict`란
	    - `TensorDict`는 텐서를 키(Key) 기반으로 관리할 수 있는 범용적인 데이터 저장 구조
	    - 단순한 텐서(Plain Tensor)를 사용하는 대신 `TensorDict`를 사용하면 간단한 데이터와 복잡한 데이터 구조를 동일하게 다룰 수 있음
	    - 매우 범용적으로 설계되어 있어, 다양한 데이터 형식을 처리하는 어려움을 제거해 줌

- TorchRL의 환경은 사용자 친화적이고 통합된 데이터 관리 방식을 제공하는 Tensordict 구조로 설계되어 있음
- `TensorDict`를 적극적으로 활용함으로써 복잡성을 줄이면서 다양한 환경에서 효율적으로 작업할 수 있도록 함
- `TensorDict` 데이터 형식: **TED (TorchRL Episode Data format)**
  - TorchRL에서 데이터를 표현하는 통일된 방식
```python
reset = env.reset()
print(reset)
# TensorDict(
#     fields={
#         done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)
#     },
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```
- `Tensordict` 내 각 필드 설명 
    - `fields`
	    - `done`: 에피소드 종료 여부(Boolean)
	    - `observation`: 환경의 관측값
	    - `terminated`: 정상 종료 여부(Boolean)
	    - `truncated`: 시간 제한 등으로 인한 중단 여부(Boolean)
    - `batch_size`
	    - 여러 에피소드를 동시에 실행하는 경우에는 `batch_size`가 에피소드 수를 나타내는 크기로 설정
	    - `torch.Size([])`: 단일 에피소드 실행을 의미
	- `device`
	    - `None`: 텐서가 저장된 장치 미지정
	    - 기본적으로 CPU 사용
	- `is_shared`
	    - `False`: 다중 프로세스 간 데이터 공유되지 않음
	    - 필요 시 `True`로 설정 가능

- 이제 action space에서 랜덤한 행동을 수행
```python
reset_with_action = env.rand_action(reset)
print(reset_with_action)
# TensorDict(
#     fields={
#         action: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.float32, is_shared=False),
#         done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)
#     },
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```
- `TensorDict` 내 각 필드 설명 
    - `fields`
	    - `action`: 수행한 행동(action) 정보
- 일반적인 딕셔너리(dictionary)를 사용하는 것처럼 쉽게 행동(action)에 접근할 수 있음
```python
print(reset_with_action["action"])
# tensor([1.5947])
```

- 이제 이 행동을 환경에 전달할 수 있음
```python
stepped_data = env.step(reset_with_action)
print(stepped_data)
# TensorDict(
#     fields={
#         action: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.float32, is_shared=False),
#         done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         next: TensorDict(
#             fields={
#                 done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#                 reward: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.float32, is_shared=False),
#                 terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)},
#             batch_size=torch.Size([]),
#             device=None,
#             is_shared=False
#         ),
#         observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)
#     },
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```
- 새로운 `TensorDict`의 특징
    - 이전 `TensorDict`와 동일하지만 `next` 항목이 추가됨
    - `next` 항목은 관측값(observation), 보상(reward), 종료 상태(done state)를 포함하는 `TensorDict`

- `next` 항목을 다음 단계로 연결하는 방법
    - 다음 단계 수행을 위해 `next` 항목만 가져와 활용할 수 있음
    - 이를 위해 TorchRL은 `step_mdp()` 함수를 제공  
        - 즉, MDP (Markov Decision Proces)의 한 단계 수행 이후 관측값에 해당하는 데이터 구조 반환

```python
from torchrl.envs import step_mdp

data = step_mdp(stepped_data)
print(data)
# TensorDict(
#     fields={
#         done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)
#     },
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```

### 3. TorchRL Environment rollouts
- 지금까지 세 가지 단계(행동 계산, 스텝 수행, MDP 내 이동)를 직접 수행하는 과정을 소개함 
- TorchRL은 이와 같은 과정을 반복적으로 실행할 수 있는 `rollout()` 함수를 제공함
```python
rollout = env.rollout(max_steps=10)
print(rollout)
# TensorDict(
#     fields={
#         action: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
#         done: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#         next: TensorDict(
#             fields={
#                 done: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 observation: Tensor(shape=torch.Size([10, 3]), device=cpu, dtype=torch.float32, is_shared=False),
#                 reward: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
#                 terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
#             },
#             batch_size=torch.Size([10]),
#             device=None,
#             is_shared=False
#         ),
#         observation: Tensor(shape=torch.Size([10, 3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
#     },
#     batch_size=torch.Size([10]),
#     device=None,
#     is_shared=False
# )
```
- 이 데이터는 `stepped_data`와 유사하지만, `max_steps` 인수를 통해 설정된 스텝 수에 따라 batch-size가 상이함
- 단일 전이(transition)에 관심이 있는 경우, 텐서를 인덱싱하는 방식으로 TensorDict를 인덱싱하여 필요한 데이터를 추출 가능
```python
transition = rollout[3]
print(transition)
# TensorDict(
#     fields={
#         action: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.float32, is_shared=False),
#         done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         next: TensorDict(
#             fields={
#                 done: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#                 reward: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.float32, is_shared=False),
#                 terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)
#             },
#             batch_size=torch.Size([]),
#             device=None,
#             is_shared=False
#         ),
#         observation: Tensor(shape=torch.Size([3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)},
#     batch_size=torch.Size([]),
#     device=None,
#     is_shared=False
# )
```
- `TensorDict`는 제공된 인덱스가 **키(key)**인지 확인하여 키 차원(key-dimension)을 따라 인덱싱하거나, 그렇지 않은 경우 공간 인덱스(spatial index)로 처리
- `rollout` 메서드는 정책(policy) 없이 실행되면 무작위 행동만 수행하기 때문에 처음에는 쓸모없어 보일 수 있음
    - 처음에는 정책 없이 rollout을 실행해 환경에서 기대할 수 있는 동작을 간단히 확인하는 데 유용할 수 있음
- 하지만, 정책이 있다면 이를 rollout 메서드에 전달하여 데이터 수집에 활용할 수 있음
    - 즉, rollout 메서드는 TorchRL API의 다재다능함을 보여줌

### 4. TorchRL Environment 변환
- 대부분의 경우 환경의 출력을 사용자의 요구에 맞게 수정해야 할 필요가 있음. 예를 들어, 
    - 마지막 reset 이후 실행된 스텝 수를 모니터링
    - 이미지를 리사이즈
    - 연속적인 관측값을 함께 스택(stack)
- 변환은 기존 환경에 `TransformedEnv`를 통해 통합

```python
from torchrl.envs import StepCounter, TransformedEnv

transformed_env = TransformedEnv(env, StepCounter(max_steps=10))
rollout = transformed_env.rollout(max_steps=100)
print(rollout)
# TensorDict(
#     fields={
#         action: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
#         done: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#         next: TensorDict(
#             fields={
#                 done: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 observation: Tensor(shape=torch.Size([10, 3]), device=cpu, dtype=torch.float32, is_shared=False),
#                 reward: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.float32, is_shared=False),
#                 terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#                 truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
#             },
#             batch_size=torch.Size([10]),
#             device=None,
#             is_shared=False
#         ),
#         observation: Tensor(shape=torch.Size([10, 3]), device=cpu, dtype=torch.float32, is_shared=False),
#         terminated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False),
#         truncated: Tensor(shape=torch.Size([10, 1]), device=cpu, dtype=torch.bool, is_shared=False)
#     },
#     batch_size=torch.Size([10]),
#     device=None,
#     is_shared=False
# )
```

- `TransformedEnv`의 인수로 `step_count`라는 항목이 추가됨 
    - 이 항목은 마지막 `reset` 이후 실행되는 스텝 수를 추적합니다.
- 변환 생성자(transform constructor)에 `max_steps=10`이라는 선택적 인수를 전달했기 때문에, 경로가 10단계 이후에 잘림 (즉, `rollout` 호출 시 요청했던 100단계 전체를 완료하지 않음).
- 경로가 잘렸는지는 `truncated` 항목을 통해 확인 가능
