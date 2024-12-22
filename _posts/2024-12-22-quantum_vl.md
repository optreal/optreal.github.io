---
layout: post
title: Quantum Virtual Link Generation via Reinforcement Learning
description: Quantum Virtual Link Generation via Reinforcement Learning
tags: Quantum, Reinforcement Learning
categories: Quantum Networks
tabs: true
related_publications: true
toc:
  sidebar: left
---

### Abstract

- Quantum networks leverage quantum entanglement as a fundamental building block. 
- When two qubits are entangled, their states exhibit non-classical correlations, enabling novel applications such as quantum key distribution and distributed quantum computing, which are not possible with classical communication. 
- However, <u>quantum entanglement is a probabilistic process heavily dependent on the characteristics of the involved devices</u>, such as optical fibers, lasers, and quantum memories. 
- Managing this process to maintain entanglement with high quality for as long as possible is a <strong>stochastic control problem</strong>.
- This process can be modeled as a MDP and solved using the RL framework. 
- In this work, we employ RL to develop an <strong>entanglement management policy</strong> that surpasses the current State-of-the-Art policies, particularly in scenarios where precise models of the quantum devices are unavailable.
- Reference: {% cite a10207249 %}


{% include figure.liquid loading="eager" path="assets/img/blogs/2024-12-22-quantum_vl/link.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}

---

### Introduction

In the last years, the application of quantum physics principles to computer networks is gaining momentum among the research and industry communities, as shown by the first attempts of standardisation of a so-called “Quantum Internet” {% cite rfc9340 %}, {% cite rfc9583 %} by the Internet Engineering Task Force (IETF).

Amongst these principle the quantum entanglement has been identified as fundamental resource for Quantum Communication {% cite rfc9340 %}, since it enables the Quantum Internet applications, as secure cryptographic key distribution and, distributed quantum computing {% cite rfc9583 %}.

But, quantum entanglement is a probabilistic process strongly dependent on the features of the
involved communication devices. Consequently, the entanglement management constitutes a stochastic control
problem that can be formulated as a Markov Decision Process (MDP) [3]. In this preliminary work, we investigate
the capacity of Deep Reinforcement Learning (DRL) to solve these problems, in particular, when a quantum
entanglement is set up between two remote communication nodes not directly connected by a link. In the
paragraphs below, we will introduce the required background.

Qubit and entanglement. In quantum communication and quantum computing, the counterpart of a classical
bit is the quantum bit (or qubit). But, whereas the classic bit can take either the “0” state or the “1” state,
the qubit can be in a superposition of the two, with a certain probability to be at one of the states. The qubit
exists in this superposition until its eventual measurement. Afterwards, it will take the “0” value or “1” value
according to the corresponding probability. When two qubits are entangled, their individual states cannot be
described in a separated way: a state change, i.e., a qubit reading measurement, in one of them implicitly comes
with a change in the other one, regardless of the physical distance between them. Thus, the measurements at
the two entangled qubits exhibit non-classical correlations used to design new applications not possible with
classical communication, such us quantum key distribution or distributed quantum computation.
Quantum network. A set of nodes able to exchange qubits and distribute entangled states amongst themselves
is defined as a quantum network in the RFC [1]. These quantum nodes are connected each other by optical
fiber or satellite laser links. In this paper, we assume fiber links. When, an entanglement is set up between
two qubits located at two adjacent quantum nodes connected by a direct link (e.g., between nodes A and B in
Fig. 1), the entanglement constitues an elementary quantum link [1]. Its success probability Pe exponentially
decreases with distance, which means that short-distance entanglements (like A-B, in Fig. 1) are more likely to
succeed than long-distance entanglements (like A-C, in Fig. 1). To overcome this issue, we can create a virtual
link [1] over two elementary links via the so-called entanglement swapping [1], [4]. This process allows creating
long-distance entangled pairs by consuming the previously generated elementary links on the path between two
further end-points. In Fig. 1, the elementary links A-B and B-C are consumed to create a longer virtual link
A-C. Quantum nodes (as B in Fig. 1) that create long-distance entangled pairs via entanglement swapping
are called quantum repeaters [1] and they must store intermediate elementary links on the so-called quantum
memories [1] to be consumed later.
Quantum memory lifetimes. The probability that a qubit stored in a quantum memory is still, after a certain
time, in its original state (e.g., an entangled state) decreases with time [5]. This probability is referred as to
memory efficiency ηm [5], and its decay is known as decoherence. This process is the consequence of the
progressive interactions of the quantum memory with the environment, since a memory cannot be perfectly
isolated from it. The entanglement swapping success probability Ps depends on the memory efficiency ηm of
the oldest loaded quantum memory involved in the swapping [6].
This paper, as far as we know, is one of the first works modelling a quantum virtual link generation process
as a classical MDP and using a DRL algorithm to find an optimal generation policy tracking the age of the
elementary links. This supposes an innovative contribution with respect to the related works, where this age of
the elementary are not used in the generation procedures.
Related works are presented in Section 2. The MDP modelling the virtual link generation along with the DRL
approach used to solve it are described in Section 3. Numerical results and experiment settings are shown in
Section 4. Section 5 concludes the article.