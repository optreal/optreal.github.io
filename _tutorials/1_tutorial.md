---
layout: page
title: TorchRL
description: Pytorch TorchRL
img: assets/img/12.jpg
importance: 1
category: reinforcement learning
related_publications: true
---

## Creating an environment
- TorchRL은 직접적으로 환경을 제공하지 않고, 대신 다른 환경 시뮬레이터 라이브러리들을 위한 wrapper 제공 
- `torchrl.envs` 모듈의 역할 
  - 일반적인 환경 API를 제공
  - 다음과 같은 simulation backend의 중앙 허브 역할 수행
    - **BraxWrapper**,
    - **DMControlEnv**,
    - **DMControlWrapper**,
    - **GymEnv**,
    - **GymWrapper**,
    - **HabitatEnv**,
    - **IsaacGymEnv**,
    - **IsaacGymWrapper**,
    - **JumanjiEnv**,
    - **JumanjiWrapper**,
    - **MeltingpotEnv**,
    - **MeltingpotWrapper**,
    - **MOGymEnv**,
    - **MOGymWrapper**,
    - **MultiThreadedEnv**,
    - **MultiThreadedEnvWrapper**,
    - **OpenMLEnv**,
    - **OpenSpielEnv**,
    - **OpenSpielWrapper**,
    - **PettingZooEnv**,
    - **PettingZooWrapper**,
    - **RoboHiveEnv**,
    - **set_gym_backend**,
    - **SMACv2Env**,
    - **SMACv2Wrapper**,
    - **UnityMLAgentsEnv**,
    - **UnityMLAgentsWrapper**,
    - **VmasEnv**,
    - **VmasWrapper**,
- 환경을 생성하는 과정 예제

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
