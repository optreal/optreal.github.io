---
layout: page
title: Offline RL Papers
date: 2025-02-20 15:00:00+0900
description: Offline Reinforcement Learning Papers
img: assets/img/3.jpg
importance: 1
category: Offline RL
related_publications: false
toc:
  sidebar: left
---

<table class="table table-responsive table-hover">
    <thead class="thead-light">
    <tr>
        <th scope="col" style="width:4%">#</th>
        <th scope="col" style="width:10%">Date</th>
        <th scope="col" style="width:32%">Book Presentation</th>
        <th scope="col" style="width:32%">Paper Presentation</th>
        <th scope="col" style="width:22%">Notice</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <th scope="row">01</th>
        <td>03월 03일(월)</td>
        <td>
            - 강의 소개<br/>
        </td>
        <td>
        </td>
        <td></td>
    </tr>
    <tr>
        <th scope="row">02</th>
        <td>03월 10일(월)</td>
        <td>
            - Deep Q Network (DQN) <br/>
            - Double DQN (DDQN) <br/>
            <!-- - Implicit Quantile Networks (QR-DQN)
             <a href="https://www.dropbox.com/scl/fi/0avmjtqv453rvgr147eak/02.Q_Learning.pdf?rlkey=xgib8swsfnloomz59sj2zrbla&dl=0" target="_blank">
                <span class="badge badge-warning">강의 노트</span>
            </a> -->
        </td>
        <td>
            - <a href="https://arxiv.org/abs/1312.5602" target="_blank">Playing Atari with Deep Reinforcement Learning</a><br/>
            - <a href="https://arxiv.org/abs/1509.06461" target="_blank">Deep Reinforcement Learning with Double Q-learning</a><br/>
            <!-- - <a href="https://arxiv.org/abs/1806.06923" target="_blank">Implicit Quantile Networks for Distributional Reinforcement Learning</a> -->
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <th scope="row">03</th>
        <td>03월 17일(월)</td>
        <td>
            - Deep Deterministic Policy Gradient (DDPG)<br/>
            - Twin Delayed Deep Deterministic Policy Gradient (TD3)
            <!-- <a href="https://www.dropbox.com/scl/fi/2nm6gt3fbzphxpjn567as/03.DQN.pdf?rlkey=mwwfnqnqzxbi6sn00x32c89jc&dl=0" target="_blank">
                <span class="badge badge-warning">강의 노트</span>
            </a> -->
        </td>
        <td>
            - <a href="https://arxiv.org/abs/1509.02971" target="_blank">Continuous Control with Deep Reinforcement Learning</a>
            - <a href="https://arxiv.org/abs/1802.09477" target="_blank">Addressing Function Approximation Error in Actor-Critic Methods</a>
        </td>
        <td>
            <!-- <span class="font-weight-bold">
                Homework #1.
                <a href="https://www.dropbox.com/scl/fi/iyat052w8oous1p148f9g/HW_1.pdf?rlkey=qggwkbwvkz7ihbutnk247nvrq&dl=0" target="_blank">
                    <span class="badge badge-primary">숙제 설명</span>
                </a>
                <br/>
                기한: 2024년 3월 31일 23시 59분
            </span> -->
        </td>
    </tr>
    <tr>
        <th scope="row">04</th>
        <td>03월 24일(월)</td>
        <td>
            - Soft Actor-Critic (SAC)
            <!-- <a href="https://www.dropbox.com/scl/fi/gjhfhn1sa8vzfuk2pjygn/05.A2C.pdf?rlkey=kl8637ey0iu3jrgb7iz6awvam&dl=0" target="_blank">
                <span class="badge badge-warning">강의 노트</span>
            </a> -->
        </td>
        <td>
            - <a href="https://arxiv.org/abs/1812.05905" target="_blank">Soft Actor-Critic Algorithms and Applications</a>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <th scope="row">05</th>
        <td>03월 31일(월)</td>
        <td>
            - Imitation Learning (IL)
        </td>
        <td>
            - <a href="https://ieeexplore.ieee.org/document/10602544" target="_blank">A Survey of Imitation Learning: Algorithms, Recent Developments, and Challenges</a><br/>
<!--
            - 논문 발표 01: <a href="https://arxiv.org/abs/1805.01954" target="_blank">Behavioral Cloning from Observation</a><br/>
            - 논문 발표 02: <a href="https://arxiv.org/abs/2109.00137" target="_blank">Implicit Behavioral Cloning</a>
-->
        </td>
        <td>
            <div style="margin-left: 1.0em">
                <strong>Imitation Learning의 목표</strong>
                <ol>
                    <li>시연을 통해 에이전트가 특정 작업이나 행동을 학습하도록 함.</li>
                    <li>시연 데이터는 **관찰(observations)**과 **행동(actions)** 간의 매핑을 학습하는 데 사용.</li>
                </ol>
            </div>
            - <a href="https://optreal.github.io/blog/2025/imitation_learning/" target="_blank">참고 자료</a>
        </td>
    </tr>
    <tr>
        <th scope="row">06</th>
        <td>04월 07일(월)</td>
        <td>
            - Batch Constrained Q-learning (BCQ) <span class="badge badge-info">ICML2019</span> <span class="badge badge-warning">Offline only</span><br/>
            - Bootstrapping Error Accumulation Reduction (BEAR) <span class="badge badge-info">NIPS2019</span> <span class="badge badge-warning">Offline only</span>
        </td>
        <td>
            - 논문 발표 01: <a href="https://arxiv.org/abs/1812.02900" target="_blank">Off-Policy Deep Reinforcement Learning without Exploration</a><br/>
            - 논문 발표 02: <a href="https://arxiv.org/abs/1906.00949" target="_blank">Stabilizing Off-Policy Q-Learning via Bootstrapping Error Reduction</a>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <th scope="row">07</th>
        <td>04월 14일(월)</td>
        <td>
            - Conservative Q-Learning (CQL) <span class="badge badge-info">NIPS2020</span> <span class="badge badge-warning">Offline and Offline-to-Online</span><br/>
            - Policy in Latent Action Space (PLAS) <span class="badge badge-info">CoRL2020</span> <span class="badge badge-warning">Offline only</span>
        </td>
        <td>
            - 논문 발표 03: <a href="https://arxiv.org/abs/2006.04779" target="_blank">Conservative Q-Learning for Offline Reinforcement Learning</a><br/>
            - 논문 발표 04: <a href="https://arxiv.org/abs/2011.07213" target="_blank">PLAS: Latent Action Space for Offline Reinforcement Learning</a>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <th scope="row">08</th>
        <td>04월 21일(월)</td>
        <td>
            - Critic Regularized Regression (CRR) <span class="badge badge-info">NIPS2020</span> <span class="badge badge-warning">Offline only</span> <br/>
            - Advantage Weighted Actor-Critic (AWAC) <span class="badge badge-info">Rejected from ICLR2021</span> <span class="badge badge-warning">Offline and Offline-to-Online</span>
        </td>
        <td>
            - 논문 발표 05: <a href="https://arxiv.org/abs/2006.15134" target="_blank">Critic Regularized Regression</a><br/>
            - 논문 발표 06: <a href="https://arxiv.org/abs/2006.09359" target="_blank">AWAC: Accelerating Online Reinforcement Learning with Offline Datasets</a><br/>
        </td>
        <td></td>
    </tr>
    <tr>
        <th scope="row">09</th>
        <td>04월 28일(월)</td>
        <td>
            - TD3+BC <span class="badge badge-info">NIPS2021</span> <span class="badge badge-warning">Offline only</span><br/>
            - Implicit Q-Learning (IQL) <span class="badge badge-info">ICLR2022</span> <span class="badge badge-warning">Offline and Offline-to-Online</span>
        </td>
        <td>
            - 논문 발표 07: <a href="https://arxiv.org/abs/2106.06860" target="_blank">A Minimalist Approach to Offline Reinforcement Learning</a><br/>
            - 논문 발표 08: <a href="https://arxiv.org/abs/2110.06169" target="_blank">Offline Reinforcement Learning with Implicit Q-Learning</a>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <th scope="row">10</th>
        <td>05월 05일(월)</td>
        <td colspan="3" class="center">공휴일 (휴강)</td>
    </tr>
    <tr>
        <th scope="row">11</th>
        <td>05월 12일(월)</td>
        <td>
            - ReBRAC <span class="badge badge-info">NIPS2023</span> <span class="badge badge-warning">Offline and Offline-to-Online</span><br/> 
            - Policy Regularization with Dataset Constraint (PRDC) <span class="badge badge-info">ICML2023</span>
        </td>
        <td>
            - 논문 발표 09: <a href="https://arxiv.org/abs/2305.09836" target="_blank">Revisiting the Minimalist Approach to Offline Reinforcement Learning</a><br/>
            - 논문 발표 10: <a href="https://arxiv.org/abs/2306.06569" target="_blank">Policy Regularization with Dataset Constraint for Offline Reinforcement Learning</a><br/>
        </td>
        <td></td>
    </tr>
    <tr>
        <th scope="row">12</th>
        <td>05월 19일(월)</td>
        <td>
            - Supported Policy OpTimizatio (SPOT) <span class="badge badge-info">NIPS2022</span> <span class="badge badge-warning">Offline-to-Online only</span><br/>
            - Calibrated Q-Learning (Cal-QL) <span class="badge badge-info">NIPS2023</span> <span class="badge badge-warning">Offline-to-Online only</span>
        </td>
        <td>
            - 논문 발표 11: <a href="https://arxiv.org/abs/2202.06239" target="_blank">Supported Policy Optimization for Offline Reinforcement Learning</a><br/>
            - 논문 발표 12: <a href="https://arxiv.org/abs/2303.05479" target="_blank">Cal-QL: Calibrated Offline RL Pre-Training for Efficient Online Fine-Tuning</a>
        </td>
        <td>
            <!--Homework #2.
                <a href="https://www.dropbox.com/scl/fi/wdas1lo3l3bsx1hhp2x6z/HW_2.pdf?rlkey=8atvaerw5mydoitb4a34x5mne&dl=0" target="_blank">
                    <span class="badge badge-primary">숙제 설명</span>
                </a>
                <br/>
                기한: 2024년 6월 9일 일요일 23시 59분
            -->
        </td>
    </tr>
    <tr>
        <th scope="row">13</th>
        <td>05월 26일(월)</td>
        <td>
            - SAC-N <span class="badge badge-info">NIPS2021</span> <span class="badge badge-warning">Offline only</span><br/>
            - Ensemble-Diversified Actor Critic (EDAC) <span class="badge badge-info">NIPS2022</span> <span class="badge badge-warning">Offline only</span><br/>
            - Large-Batch SAC (LB-SAC) <span class="badge badge-info">NIPS2022</span> <span class="badge badge-warning">Offline only</span>
        </td>
        <td>
            - 논문 발표 13: <a href="https://arxiv.org/abs/2110.01548" target="_blank">Uncertainty-Based Offline Reinforcement Learning with Diversified Q-Ensemble</a><br/>
            - 논문 발표 14: <a href="https://arxiv.org/abs/2211.11092" target="_blank">Q-Ensemble for Offline RL: Don't Scale the Ensemble, Scale the Batch Size</a>
        </td>
        <td></td>
    </tr>
    <tr>
        <th scope="row">14</th>
        <td>06월 02일(월)</td>
        <td>
            - Decision Transformer (DT) <span class="badge badge-warning">Offline only</span><br/>
        </td>
        <td>
            - 논문 발표 15: <a href="https://arxiv.org/abs/2106.01345" target="_blank">Decision Transformer: Reinforcement Learning via Sequence Modeling</a><br/>
        </td>
        <td></td>
    </tr>
    <tr>
        <th scope="row">15</th>
        <td>06월 09일(월)</td>
        <td>
            - Gato <br/>
        </td>
        <td>
            - 논문 발표 16: <a href="https://arxiv.org/abs/2205.06175" target="_blank">A Generalist Agent</a>
        </td>
        <td>
        </td>
    </tr>
    <tr>
        <th scope="row">16</th>
        <td>06월 16일(월)</td>
        <td COLSPAN="2">기말 고사</td>
        <td>
        </td>
    </tr>
    </tbody>
</table>