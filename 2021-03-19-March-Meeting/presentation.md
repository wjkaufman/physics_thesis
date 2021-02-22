---
title: Using Reinforcement Learning for Quantum Control in Magnetic Resonance [DRAFT]
author: Will Kaufman
date: February 22, 2021
---

# Quantum control in spin systems

<!--
Include relevant Hamiltonians, q control framework
 -->

$$
H(t) = H_\text{system} + H_\text{control}(t)
$$

$$
H_\text{system} &= \sum_i \delta_i I_z^i + \sum_{i,j} d_{ij} \left( 3I_z^iI_z^j - \mathbf{I^i} \cdot \mathbf{I^j} \right)
  = H_\text{CS} + H_\text{D}
$$

$$
  H_\text{control}(t) &= -B_1(t) \sum_i \gamma_n^i I_x^i
$$

- Time suspension (protecting coherent states)
- Sensing
- Novel systems

# Reinforcement learning for quantum control

<!--
Agent, environment, state, action, reward
 -->

\begin{figure}
    \centering
    \scalebox{.5}{
    \begin{tikzpicture}[->, very thick]
         %nodes
         \node[draw] at (-3,0) (agent) {Agent ($\pi: S \to \mathcal{P}(A)$)};
         \node[draw] at (3,0) (env) {Environment};
         \path
             (agent.north) edge[bend left=30] node[above] {Action $a \in A$} (env.north)
             (env.south) edge[bend left=30] node[below] {State $s \in S$} (agent.south)
             (env.west) edge node[below] {Reward $r$} (agent.east);
    \end{tikzpicture}
    }
\end{figure}

- State $\to$ density operator or propagator
- Action $\to$ control Hamiltonian $H_\text{control}(t)$
- Reward $\to$ state or operator fidelity

Can apply RL to discrete control problem (constructing pulse sequences) or continuous control (shaped pulses, continuously-varying control Hamiltonians)

# Constructing pulse sequences using RL

<!--
AlphaZero algorithm, quick summary of what it does
-->

# Computational results

Time suspension, 48-pulse sequence

Robustness against different sources of error

Comparisons to existing pulse sequences

# Experimental results

Awaiting data...

# Future work

...?

# Appendix

- Neural network structure
- Algorithm pseudocode
- Training performance, distributions of pulse sequence performance
- Exact structures of Hamiltonians (how CS/D are drawn from normal distributions)
- What else??? TODO
