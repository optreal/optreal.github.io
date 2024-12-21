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

### Creating an environment
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
- TorchRL에서 GymWrapper와 GymEnv는 각각의 사용 용도가 조금 다르며, 둘 다 Gym 환경과의 상호작용을 위해 설계되었어. 자세한 사용 용도는 다음과 같아:

#### GymEnv
- GymEnv는 Gym 환경을 TorchRL에서 직접 사용할 수 있도록 encapsulation(포장)하는 역할 수행.
- TorchRL의 기본적인 환경 API를 따르며, Gym의 환경을 그대로 활용할 수 있도록 함.
- 주로 Gym 환경을 처음 생성하거나 로드할 때 사용.
- 주요 특징:
    - TorchRL 표준화된 API를 통해 Gym 환경에 접근 가능.
    - reset, step, render와 같은 메서드가 제공됨.
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
- GymWrapper는 이미 생성된 GymEnv 또는 유사한 환경을 추가적으로 변환하거나 TorchRL 표준에 맞게 확장하기 위해 사용
- GymWrapper는 기존의 Gym 환경에 새로운 기능을 추가하거나, 입력/출력 포맷을 변경할 때 사용
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
#                 truncated: Tensor(shape=torch.Size([1]), device=cpu, dtype=torch.bool, is_shared=False)},
#             batch_size=torch.Size([]),
#             device=None,
#             is_shared=False)},
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

### 차이점 요약

| **항목** | **GymEnv** | **GymWrapper** |
| **사용 목적** | **Gym** 환경을 **TorchRL**의 표준 API로 변환 | 이미 생성된 환경에 추가적인 기능을 부여하거나 변환  |
| **적용 대상** | 기존 **Gym** 환경 | **GymEnv** 또는 유사한 환경 |
| **사용 시점** | 환경 초기 생성 단계 | 환경 생성 후 추가 변환이나 전처리가 필요할 때 |
| **주요 작업** | **reset**, **step**, **render**와 같은 기본 작업 제공 | 관측값 변환, 동작 공간 변경, 환경 속성 추가 |
{:.mbtablestyle}