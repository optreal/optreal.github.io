---
layout: post
title: "Enhancing Safety via Deep Reinforcement Learning in Trajectory Planning for Agile Flights in Unknown Environments"
description: "Enhancing Safety via Deep Reinforcement Learning in Trajectory Planning for Agile Flights in Unknown Environments"
date: 2025-1-14 00:00:00
tags: Trajectory-Planning, Reinforcement-Learning
categories: Drone
tabs: true
toc:
  sidebar: left
---

<div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/traj_planning/authors.png" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

## Abstract
- Motivation
  - Increase necessary to swiftly evade obstacles and adapt trajectories under hard real-time constraints.
    - Generate viable paths that prevent collisions while maintaining high speeds with minimal tracking errors.

- Method
  - The proposed method combines a supervised learning approach, as teacher policy, with deep reinforcement learning (DRL), as student policy.
  1. Train the teacher policy using a path planning algorithm that prioritizes safety while minimizing jerk and flight time.
  2. Use this policy to guide the learning of the student policy in various unknown environments.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/traj_planning/framework.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

### Introduction

- Previous works' limitation
  - Require
    - Known environments.
    - Extensive information for reliable outcomes, which is seldom the case in real-world missions.

- Proposed Method
  - A trajectory planning method for generating agile flight trajectories in unknown environments solely based on data from onboard sensors.
  - The framework integrates two neural networks:
    - The teacher policy
      - Supervised learning.
      - Incorporates a geometry-based trajectory planning strategy enriched with a heuristic to optimize flight time and enhance safety.
    - The student policy
      - DQN-PER.

***

## Methodology

- The primary aim
  - The primary aim of our proposed approach is to facilitate the safe and agile navigation of UAVs in **unknown environments**, leveraging **3D LiDAR** for environmental perception.
  - Our strategy involves establishing the UAV trajectory with a minimal flight time based on real-time sensory data while prioritizing safety.

- The proposed privileged reinforcement learning framework
  - The teacher policy
    - Deep Feedforward Neural Network (DFNN).
    - Trains using an expert algorithm proficient. 
    - Provides optimal action insights across diverse environments.
    - Evaluates the student policy **(DRL reward)**.
  - The student policy
    - DQN-PER
    - Operates based on data identifiable by the UAV’s 3D Lidar around its current pose.
  - Both networks output the new ideal waypoints to be followed ($$F$$, $$B$$, $$R$$, $$L$$, $$U$$, $$D$$).
  - The distilled knowledge from the teacher policy is integrated into the student policy, functioning without privileged information.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/traj_planning/framework.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

#### Problem Definition
- Our focus is on formulating a practical and reliable trajectory planning strategy, aiming to optimize three key objectives:
  - Minimizing jerk.
  - Reducing flight time.
  - Enhancing safety by maximizing the distance to the obstacles in the environment.
- The goal is to minimize equations (1), (2), and (3) while maximizing (4) to enhance safety and ensure collision free trajectories for high-speed flights.
  1. Goal Distance:
  $$
  D_{\text{goal}} = \sum_{i=0}^n \sqrt{(\mathbf{p}_{\text{goal}} - \mathbf{p}_i)^2}
  $$
  2. Next Step Distance:
  $$
  D_{\text{next}} = \sum_{i=0}^{n-1} \sqrt{(\mathbf{p}_{i+1} - \mathbf{p}_i)^2}
  $$
  3. Jerk Cost:
  $$
  D_{\text{jerk}} = \int \left( \frac{d^3 \mathbf{p}}{dt^3} \right)^2 dt
  $$
  4. Obstacle Distance:
  $$
  D_{\text{obs}} = \max \sum_{j=0}^k \sqrt{(\mathbf{p}_i - \mathbf{O}_j)^2}
  $$

### Expert

- Bidirectional A* algorithm
  - We integrate objective equations as heuristics for trajectory generation:
$$
f(i) = (D_{\text{goal}} + D_{\text{obs}} + D_{\text{next}}) \times D_{\text{jerk}}
$$
- Input: 
  - Entire environment.
  - Global position.
  - Goal Node.
- Output: 
  - The ideal next waypoint for each waypoint.

### Teacher Policy
- The policy is trained across multiple randomly generated scenarios.
  - Rainforests.
  - Mazes.
  - Disaster Areas.
- To ensure precision, a distinct model is learned for each scenario, resulting in 30 models trained across different scenarios, with 10 models for each environment.
- Input:
  - Goal node.
  - Global position.
  - Orientation.
  - The environment around
within a 5 × 5 × 2 meter radius range.
- Output:
  - ($$F$$, $$B$$, $$R$$, $$L$$, $$U$$, $$D$$).
  - The subsequent ideal action determined by our expert.

### Student Policy
- The student policy efficiently produces real-time,
collision-free trajectories for agile flights, relying solely
on onboard sensor measurements.
- Input:
  - obstacle positions within a 5 × 5 × 2 meter radius
obtained from the 3D Lidar.
  - UAV position
  - Orientation.
  - Goal node.
- Ouput:
  - ($$F$$, $$B$$, $$R$$, $$L$$, $$U$$, $$D$$).
- During flight, the UAV records obstacle positions, triggering policy execution upon identifying new obstacles.
- The policy generates a new trajectory using obstacle positions providing a single waypoint per iteration.
- Multiple policy runs are conducted to create the final trajectory.
- Upon generating a trajectory, it utilizes Bézier curves to transform the anticipated trajectory into a comprehensive state representation.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/traj_planning/framework.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>

## Results
- The proposed method is evaluated based on:
  - Flight time (seconds)
  - Processing time (seconds)
  - Representing the time
the UAV is at least at 80% of the maximum speed
  - RMSE from the guidance trajectory (meters)
  - Success rate (%)
- Baseline:
  - the standard bidirectional A* in an unknown environment
  - our expert operating in an unknown environment
  - The original DQN-PER
  - Our teacher policy
- Testing in simulation demonstrates noteworthy advancements, including an 80% reduction in tracking error, a 31% decrease in flight time, a 19% increase in high-speed duration, and a success rate improvement from 50% to 100%, as compared to baseline methods.

<div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/traj_planning/results.png" class="img-fluid rounded z-depth-1" zoomable=true %}
</div>


