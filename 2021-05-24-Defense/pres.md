---
  # To render into reveal.js slides: `pandoc --mathjax --citeproc -t revealjs -s pres.md -o pres.html`
  
  title: Applying Reinforcement Learning to Hamiltonian Engineering
  author: Will Kaufman
  date: May 24, 2021
  
  bibliography: ../refs.bib
  
  theme: white
  transition: none
  slideNumber: true
  progress: false
  width: 1920
  height: 1080
---

<h1>Applying Reinforcement Learning to Hamiltonian Engineering</h1>

Will Kaufman

May 24, 2021

![image](../graphics/qisD.png){ height=150px }\ <span style="padding-left:20px"></span> ![image](../graphics/Dartmouth.png){ height=140px }\

<style>
.reveal h1, .reveal h2, .reveal h3, .reveal h4, .reveal h5 {
  text-transform: none;
}
</style>

## Hamiltonian engineering

The system Hamiltonian is the *generator* of time evolution: determines
how system changes in time

$$H(t) = H_{\text{sys}} + H_{\text{ctrl}}(t)$$

Goal: engineer a Hamiltonian $H_{\text{target}}$ different from
$H_{\text{sys}}$ using $H_{\text{ctrl}}(t)$

![From @quadcopter.](../graphics/quadcopter.jpg){width=40%}


::: notes

Hamiltonian engineering used for:

-   Sensing (decoupling dipolar interactions while keeping chemical
    shift, e.g.
    $H_\text{target} = \beta \textcolor[rgb]{0,0.69,0}{H_{\text{CS}}}$)

-   Quantum memory (decoupling all interactions, $H_\text{target} = 0$)

-   Quantum simulation

:::

## System: spin-1/2 particles and spin systems

![](../tikz/Spin-System.png){width=60%}\

$$
H_{\text{sys}} =\sum _{i}\delta_{i} I^{i}_{z}
    + \sum _{i,j}d_{ij}\left( 3I^{i}_{z} I^{j}_{z} -\mathbf{I^{i}} \cdot \mathbf{I^{j}}\right)
    = H_{\text{CS}} + H_{D}
$$

::: notes

Only focusing on closed system dynamics with global control (spins are
not addressed individually).

:::

## Control: collective rotation of spins

![](../tikz/Single-Spin-Control.png){width=25%}\

$B_x$ and $B_y$ are the "knobs" we can turn to interact with the spin
system

$$H_{\text{ctrl}}(t) = -B_x(t) \sum_i \gamma_n^i I_x^i -B_y(t) \sum_i \gamma_n^i I_y^i$$

Can apply **$\pi/2$-pulses** that collectively rotate spins $90^\circ$

## State, dynamics, measurement

![](../tikz/NMR-Experiment.png){width=80%}\

## Hamiltonian engineering application: magnetic dipolar interactions

<div style="width: 50%; float: left;">
![From Facey 2008.](../graphics/1H-NMR.jpg){height=450px}
</div>
<div style="width: 50%; float: left;">
![](../tikz/Bath-Spins.png){height=450px}
</div>
<div style="clear: both;"></div>

Magentic dipolar interactions ($H_D$) are not wanted...

-   Broaden spectral lines in NMR

-   Lead to decay of central spin coherence in bath (NV/P1 centers,
    semiconductor qubit)

Decoupling them would narrow spectral lines and increase coherence times.

::: notes

Talk about bath engineering more, give NV/P1 center example

:::

## Correlation functions

$C_{XX}(t)$: Initialize state along $x$, measure magnetization along $x$
over time

$$C_{avg}(t) = (C_{XX}(t) C_{YY}(t) C_{ZZ}(t))^{(1/3)}$$

Decoupling dipolar interactions slows decay of correlation function

![](../graphics/decay_plot_0.png){width=45%}

::: notes

Plot of average correlation of adamantane sample for FID, and using two
pulse sequences. See how coherence time improves with dipolar
decoupling! But there's still room to improve before we reach T1 limit.

:::

## Average Hamiltonian theory (AHT)

Given how we turn the "knobs" for $H_\text{ctrl}(t)$, we can calculate
what Hamiltonian we engineered with AHT!

The Magnus Expansion gives an exponential solution for propagator $U(t)$
via an *average Hamiltonian* $\overline{H}$ at time $t$
[@Blanes_2009; @Magnus_Pedagogical] $$U(t) = e^{-i \overline{H} t}$$
with $\overline{H} = \overline{H}^{(0)} + \overline{H}^{(1)} + \dots$.

$$\overline{H}^{(0)} = \frac{1}{T} \int_0^{T}
    H(t) dt$$

The series converges rapidly when $||H||t \ll 1$.

## AHT: pulse sequences

If $H_{\text{ctrl}}$ is cyclic on the interval $[0,T]$ and periodic
[@gerstein-dybowski]

<!-- $$ -->
\begin{align}
    U_{\text{ctrl}}(T) &= \pm 1\, \text{(cyclic)} \\
    % T\exp \left( -i \int_0^{T} H_{\text{ctrl}}(t) dt \right) =
    H_{\text{ctrl}}(t) &= H_{\text{ctrl}}(t + NT) \, \text{(periodic)}
\end{align}
<!-- $$ -->

then the time evolution over $[NT, (N+1)T]$ is characterized by
$\overline{H}$

![](../tikz/WHH-4.png){width=60%}

## WAHUHA-4 pulse sequence

WAHUHA 4-pulse sequence decouples dipolar interaction to lowest-order
while keeping chemical shifts [@PhysRevLett.20.180]

$$H_{\text{CS}} + H_D \longrightarrow H_{\text{CS}}'$$

![](../tikz/AHT-Diagram.png){width=70%}

::: notes

WHH-4 designed with pen and paper using AHT

Average Hamiltonian Theory (AHT) has been used extensively for
Hamiltonian engineering. AHT lets you design pulse sequences that
decouples (cancels) certain terms in the Hamiltonian.

Axes in the figure show the orientation of the interaction frame, cyclic
because first and last orientations are the same.

If you measure at $T$, then system will appear to evolve under
$\overline{H}$.

:::

## CORY48 pulse sequence

CORY 48-pulse sequence [@CORY1990205] designed analytically using AHT
to decouple *all* interactions to second order, robust to experimental
errors

$$H_{\text{CS}} + H_D \longrightarrow 0$$

<!-- $$ -->
\begin{align}
    X, \tau, Y, 2\tau, \overline{X}, \tau, Y, 2\tau, X, \tau, Y, 2\tau,
    X, \tau, Y, 2\tau, X, \tau, \overline{Y}, 2\tau, X, \tau, Y, 2\tau \\
    %
    \overline{Y}, \tau, \overline{X}, 2\tau, Y, \tau, \overline{X}, 2\tau,
    \overline{Y}, \tau, \overline{X}, 2\tau, \overline{Y}, \tau, \overline{X},
    2\tau, \overline{Y}, \tau, X, 2\tau, \overline{Y}, \tau, \overline{X},
    2\tau \\
    %
    \overline{X}, \tau, Y, 2\tau, \overline{X}, \tau, \overline{Y}, 2\tau,
    \overline{X}, \tau, Y, 2\tau, X, \tau, \overline{Y}, 2\tau, \overline{X},
    \tau, \overline{Y}, 2\tau, X, \tau, \overline{Y}, 2\tau \\
    %
    Y, \tau, \overline{X}, 2\tau, Y, \tau, X, 2\tau, Y, \tau, \overline{X}, 2\tau, \overline{Y}, \tau, X, 2\tau, Y, \tau, X, 2\tau, \overline{Y}, \tau, X, 2\tau
\end{align}
<!-- $$ -->

## AHT limitations

<!-- $$ -->
\begin{align}
    \overline{H}^{(0)} &= \frac{1}{T} \int_0^{T}
        \widetilde{H}_{\text{sys}}(t) dt \\
    %
    \overline{H}^{(1)} &= \frac{1}{2iT} \int_0^{T} dt_1 \int_0^{t_1} dt_2
        \left[\widetilde{H}_{\text{sys}}(t_1), \widetilde{H}_{\text{sys}}(t_2)\right] \\
    %
    \overline{H}^{(2)} &= -\frac{1}{6T}
    \int_0^{T} dt_1 \int_0^{t_1} dt_2 \int_0^{t_2} dt_3
    \left\{
    \left[\widetilde{H}_{\text{sys}}(t_1), \left[\widetilde{H}_{\text{sys}}(t_2), \widetilde{H}_{\text{sys}}(t_3)\right]\right] \right. \\
    & \hspace{13em} + \left.
    \left[\left[\widetilde{H}_{\text{sys}}(t_1), \widetilde{H}_{\text{sys}}(t_2)\right], \widetilde{H}_{\text{sys}}(t_3)\right]
    \right\}
\end{align}
<!-- $$ -->

And higher-order terms get even worse...

::: notes

AHT is great, but it gets really hard to calculate higher order terms
(especially for longer pulse sequences).

"RL has been advancing rapidly in recent years, useful tool for tackling
a wide range of problems"

Are there better approaches to Hamiltonian engineering using RL?

:::

## AHT limitations



Pulse sequence design with AHT has been effective, but may be approaching its limitations

<p class="fragment">*Is there a better way?*</p>



<p class="fragment">Perhaps with reinforcement learning...</p>


<!-- Reinforcement learning (RL) for Hamiltonian engineering
======================================================= -->

## Reinforcement learning

<div style="width: 65%; float: left;">

<p style="margin-bottom: 200px"></p>

![From @sutton2018reinforcement.](../graphics/rl.png){width=90%}
</div>
<div style="width: 35%; float: left;">
![](../graphics/tetris.png){width=90%}
</div>
<div style="clear: both;"></div>




## Applications of RL

![](../graphics/chess.jpg){height=500px}\ ![](../graphics/space-invaders.jpg){height=500px}\

![From @gu2016deep.](../graphics/robot.png){height=300px}


## RL for Hamiltonian engineering

-   Action $\to$ delay time $\tau$ or apply $\pi/2$-pulse
    ($\{ D, X, \overline{X}, Y, \overline{Y} \}$)

-   State $\to$ sequence of pulses applied via $H_{\text{ctrl}}$

![](../tikz/Pulse-Action.png){height=700px}\

::: notes

With RL, potential for tailored pulse sequences for particular systems
(e.g. strongly coupled vs weakly coupled), learned robustness to errors.

:::

## Hamiltonian engineering reward function

The agent is doing a good job when the propagator $U$ is close to the
target propagator $U_\text{target}$

$$\text{F}(U, U_\text{target}) = \left| \frac{\text{Tr}\left(
    U^\dagger U_\text{target}
 \right)}{\text{Tr}\left( 1 \right)} \right|$$

Because fidelity is between $0$ and $1$, use log transformation for
reward function $$r = -\log(1 - \text{F})$$

An order of magnitude improvement...
$$F=0.99 \to F=0.999, \quad r=2 \to r=3$$

## AlphaZero Algorithm

Tried *many* different RL algorithms for pulse sequence design,
ultimately used AlphaZero algorithm [@Silver1140]

Originally designed to play Chess, Shogi, and Go

![](../tikz/AZ-Parallel.png){height=650px}\


::: notes

With tree structure, can optionally add AHT constraints to search.

:::

## AlphaZero: tree search

![](../tikz/MCTS/MCTS-1-1.png){width=70%}\

## AlphaZero: tree search

![](../tikz/MCTS/MCTS-2-1.png){width=70%}\

## AlphaZero: tree search

![](../tikz/MCTS/MCTS-3-1.png){width=70%}\

## AlphaZero: tree search

![](../tikz/MCTS/MCTS-4-1.png){width=70%}\

## AlphaZero: tree search

![](../tikz/MCTS/MCTS-5-1.png){width=70%}\

## AlphaZero: tree search

![](../tikz/MCTS/MCTS-6-1.png){width=70%}\

## AlphaZero: network training

Data collected from tree search used to train network parameters
$\theta$ with loss function

$$L(\theta) = (r_i - v_\theta(s_i))^2 - \pi_\theta(s_i) \cdot \log \mathbf{p_i} + c ||\theta||^2$$

And updated parameters $\theta'$ are used in tree search...

## Computational results

Goal: decouple all interactions ($H_{\text{target}} = 0$) in strongly
coupled spin systems.

-   Consistency checks: $10$ identical runs using same parameters
-   Unconstrained tree search (*"tabula rasa"*, no AHT knowledge)
-   Constrained tree search with AHT
-   Add errors to simulations during training

Each job runs for 12 hours using 16 CPUs

## Consistency check

![](../graphics/az-consistency.png){width=60%}\

::: notes

$24\tau$ sequence, no errors, AHT0 and 6tau constraints.

Algorithm is inherently random, leads to different performance.

Lines are different lengths because jobs run for equal times, but if
policies converge faster then tree search is faster.

:::

## Unconstrained tree search

<div style="width: 33%; float: left;">
![$12\tau$ sequence](../graphics/hists/reward_hist-no_errors-no_constraints-12.png){width=100%}
</div>
<div style="width: 33%; float: left;">
![$24\tau$ sequence](../graphics/hists/reward_hist-no_errors-no_constraints-24.png){width=100%}
</div>
<div style="width: 33%; float: left;">
![$48\tau$ sequence](../graphics/hists/reward_hist-no_errors-no_constraints-48.png){width=100%}
</div>
<div style="clear: both;"></div>



Want dark bands to move down and to the right

::: notes

Distribution of "rewards" ($-\log(1 - \text{fidelity})$) during
AlphaZero training. No constraints were applied to the tree search, and
the simulated spin systems were idealized.

:::

## Constraints to tree search

AHT constraint for $\overline{H}^{(0)}$: toggle $I_z$ to
$I_x, I_y, I_z, -I_x, -I_y, -I_z$ for equal times for the entire pulse
sequence

![](../tikz/AHT-Diagram.png){height=450px}


Additional constraint: toggle $I_z$ to $I_x, I_y, I_z, -I_x, -I_y, -I_z$
*every* $6\tau$ in the pulse sequence

## Constrained tree search with AHT


<div style="width: 33%; float: left;">
![$12\tau$ sequence](../graphics/hists/reward_hist-no_errors-AHT0-12.png){width=100%}
</div>
<div style="width: 33%; float: left;">
![$24\tau$ sequence](../graphics/hists/reward_hist-no_errors-AHT0-24.png){width=100%}
</div>
<div style="width: 33%; float: left;">
![$48\tau$ sequence](../graphics/hists/reward_hist-no_errors-AHT0-48.png){width=100%}
</div>
<div style="clear: both;"></div>






Want dark bands to move down and to the right

::: notes

Distribution of "rewards" ($-\log(1 - \text{fidelity})$) during
AlphaZero training. Lowest-order AHT constraints were applied to the
tree search, and the simulated spin systems were idealized.

:::

## Constrained tree search with AHT and $6\tau$ refocusing

<div style="width: 33%; float: left;">
![$12\tau$ sequence](../graphics/hists/reward_hist-no_errors-6tau-12.png){width=100%}
</div>
<div style="width: 33%; float: left;">
![$24\tau$ sequence](../graphics/hists/reward_hist-no_errors-6tau-24.png){width=100%}
</div>
<div style="width: 33%; float: left;">
![$48\tau$ sequence](../graphics/hists/reward_hist-no_errors-6tau-48.png){width=100%}
</div>
<div style="clear: both;"></div>

Want dark bands to move down and to the right

::: notes

Distribution of "rewards" ($-\log(1 - \text{fidelity})$) during
AlphaZero training. Lowest-order AHT constraints and refocusing all
interactions every $6\tau$ were applied to the tree search, and the
simulated spin systems were idealized.

:::

## Candidate pulse sequences from training

$12\tau$ $$\begin{aligned}
    X, \tau, \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, \overline{Y}, \tau, \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, X, \tau, \overline{Y}, \tau, X, \tau, X, \tau
\end{aligned}$$

$24\tau$ $$\begin{aligned}
    \overline{Y}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, X, \tau, \overline{Y}, \tau, X, \tau, \\
    \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, X, \tau, X, \tau, \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, \overline{Y}, \tau
\end{aligned}$$

$48\tau$ $$\begin{aligned}
    \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{X}, \tau, Y, \tau, X, \tau, Y, \tau, X, \tau, X, \tau, Y, \tau, \\
    Y, \tau, X, \tau, X, \tau, Y, \tau, X, \tau, X, 2\tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \\
    \overline{X}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{Y}, \tau,
    \overline{X}, \tau, \overline{X}, \tau, \overline{X}, \tau, \overline{Y}, 2\tau, \overline{Y}, \tau, \\
    \overline{Y}, \tau, \overline{X}, \tau, \overline{Y}, \tau, \overline{Y}, \tau, X, \tau, Y, \tau, \overline{X}, \tau, Y, \tau, \overline{X}, \tau, Y, \tau
\end{aligned}$$

## Experimental errors

In reality, there are imperfections...

<div style="width: 50%; float: left;">
![Rotation errors.](../tikz/Rotation-Error.png){width=50%}
</div>
<div style="width: 33%; float: left;">
![Phase transients, (a) is ideal pulse, (b) is actual pulse with phase transients. From @1976ii.](../graphics/phase_transients_viz.png){width=100%}
</div>
<div style="clear: both;"></div>



## Robustness to rotation errors (no errors in training)

"You play like you practice..."

![](../graphics/robustness/rot_errors-no_errors.png){width=50%}

Evaluated in simulations with no errors except for rotation error.

::: notes

Robustness against rotation errors, relative to a $\pi/2$-pulse. The
pulse sequences identified using AlphaZero have very poor robustness to
rotation errors, while the CORY48 sequence (which was designed using AHT
to be robust to such errors) has high fidelity for a much broader range
of rotation errors.

:::

## Robustness to phase transient errors (no errors in training)

![](../graphics/robustness/phase_transients-no_errors.png){width=50%}

Evaluated in simulations with no errors except for phase transient
error.

## Robustness to resonance offset errors (no errors in training)

Robust to resonance offset errors (trained with chemical shifts of
similar magnitude), but fidelity is still worse than CORY48.

![](../graphics/robustness/offset_errors-no_errors.png){width=50%}

Evaluated in simulations with no errors except for resonance offset
error.

## Robustness to rotation errors (errors in training)

![](../graphics/robustness/rot_errors-all_errors.png){width=50%}

Evaluated in simulations with all errors.

## Robustness to phase transient errors (errors in training)

![](../graphics/robustness/phase_transients-all_errors.png){width=50%}

Evaluated in simulations with all errors.

## Robustness to resonance offset errors (errors in training)

![](../graphics/robustness/offset_errors-all_errors.png){width=50%}

Evaluated in simulations with all errors.

## Experimental results

![](../graphics/decay_plot_3.png){width=60%}

## Summary

-   Hamiltonian engineering is important problem in quantum physics/engineering
-   Decoupling dipolar interactions is important for narrowing linewidths, increasing coherence times
-   RL is promising new tool to design new pulse sequences
    -   Tailored control for specific system characteristics and errors
    -   Best-performing approach combination of RL and AHT
- **Next steps**
    - Further tune RL algorithm for pulse sequence search
    - Improve constraints on tree search for higher-order terms in average Hamiltonian
    - Curriculum learning, spin simulation parameters, other target Hamiltonians

## Acknowledgements

Special thanks to...

-   Benjamin Alford
-   Linta Joseph, Ethan Williams
-   Paola Cappellaro, Xiaoyang Huang, Pai Peng
-   Chandrasekhar Ramanathan

::: notes

Ben: evaluating pulse sequences,

Linta, Ethan: getting me up to speed in lab group, teaching a lot with
AHT, experiments, lots of feedback

Paola, Xiaoyang, Pai: collaborators on RL-based Hamiltonian engineering,
their work motivated my own interest in the problem, answering lots of
early questions

Sekhar: showing me the cool, exciting world of quantum physics, guiding
my research as thesis advisor

:::



## Appendix

## AHT: interaction frame

What if $t||H||$ is large (strong pulses from $H_{\text{ctrl}}$)? Go
into the interaction frame! [@brinkmann_2016]

$$\begin{aligned}
    \frac{d}{dt} U_{\text{ctrl}}(t) &=
        -i H_{\text{ctrl}}(t)U_{\text{ctrl}}(t) \\
    U_{\text{ctrl}}(0) &= 1\end{aligned}$$

So the Hamiltonian in the interaction frame becomes

$$\widetilde{H}(t) = \widetilde{H}_{\text{sys}}(t) = U_{\text{ctrl}}(t)^\dagger H_{\text{sys}} U_{\text{ctrl}}(t)$$

## AHT: Special Cases

-   Symmetric pulse sequences ($H(\tau) = H(T - \tau)$): all odd-order
    terms in average Hamiltonian are zero

-   Antisymmetric pulse sequences ($H(\tau) = - H(T - \tau)$): all terms
    in average Hamiltonian are zero

## Simulation/RL parameters

-   $N=3$ spin-1/2 system, $\delta_i \sim \mathcal{N}(0, 1)$,
    $d_{ij} \sim \mathcal{N}(0, 100)$

-   Delay $\tau = 10^{-4}$, pulse length $t_p = 10^{-5}$

-   Ensemble of 50 spin systems with different chemical shifts and
    dipolar interactions

```{=html}
<!-- -->
```
-   Replay buffer size: $10^6$ "experiences" ($(s, a, r)$)

-   Batch size: $2048$

-   Training duration: $10^4$ training steps

$$\text{fidelity}(U, U_\text{target}) = \operatorname{Re}{
        \frac{\text{Tr}\left( U_\text{target}^\dagger U(t) \right)}{\text{Tr}\left( 1 \right)}
    }$$ For RL algorithm performance, use log infidelity as "reward"
$$r = -\log \left( 1 - \text{fidelity} \right)$$

$$r = 4 \iff \text{fidelity} = 0.\underline{9999}$$

## AlphaZero: neural network structure

## Computational results: AlphaZero algorithm learns

![](../graphics/az_learning/hist-0000.png){width=".49\\textwidth"}
![](../graphics/az_learning/hist-0030.png){width=".49\\textwidth"}\
![](../graphics/az_learning/hist-0090.png){width=".49\\textwidth"}
![](../graphics/az_learning/hist-0145.png){width=".49\\textwidth"}

## Comparison between RL approaches

Different RL algorithm used by our collaborators [@peng2021deep].

  Characteristic                            Evolutionary Reinforcement Learning           AlphaZero
  ----------------------------------------- --------------------------------------------- ----------------------------------------------------
  State representation                      Sequence of previous pulses                   Same
  Action space                              Delay or $\pi/2$-pulse along $\pm$X, $\pm$Y   Same
  Learning method                           Evolutionary algorithms (gradient-free)       Tree search and experience replay (gradient based)
  Prior knowledge                           Builds longer sequences from shorter ones     Uses AHT to prune tree search
  Pulse sequences ($H_\text{target} = 0$)   yxx48                                         az48

## AlphaZero algorithm

Explore new pulse sequences

1.  Start with a zero-length pulse sequence as the root node

2.  With the given root node, perform Monte Carlo Tree Search (MCTS) to
    explore potential pulses

    MCTS uses a neural network to estimate the prior probabilities for
    selecting each pulse and the value (fidelity) for the final pulse
    sequence

3.  Sample the next pulse from the root node's children weighted by
    their visit counts

4.  Repeat steps 2-4 until a complete pulse sequence is determined

5.  Record the child nodes' visit counts and final pulse sequence
    fidelity to a data buffer for training

Parameters for MCTS, training, etc.

## AlphaZero algorithm (cont.)

Train neural networks on collected data

-   Policy loss: want to minimize the difference between MCTS visit
    counts $\mathbf{p}$ and learned policy $\pi_\theta$

-   Value loss: want to minimize the difference between calculated
    fidelity from pulse sequence $z$ and predicted fidelity from neural
    network $v$

-   L2 regularization: prevent overfitting to data

-   $l(\theta) = -\mathbf{p} \cdot \log\pi_\theta + (z - v)^2 + c||\theta||^2$

## Neural network training

![](../graphics/loss_policy.png){width=".49\\textwidth"}
![](../graphics/loss_value.png){width=".49\\textwidth"}

## Training performance

![](../graphics/training_fidelity.png){width=".8\\textwidth"}

## Pulse sequences identified using AlphaZero

[az48]{.roman} pulse sequence (decouple all interactions):

$-X, \tau, Y, \tau, Y, \tau, X, \tau, Y, \tau, Y, \tau$
$-Y, \tau, X, \tau, X, \tau, -Y, \tau, X, \tau, X, \tau$
$Y, \tau, X, \tau, X, \tau, -Y, \tau, X, \tau, X, \tau$
$-Y, \tau, X, \tau, -Y, \tau, X, \tau, X, \tau, -Y, \tau$
$-X, \tau, -X, \tau, Y, \tau, Y, \tau, -X, \tau, Y, \tau$
$Y, \tau, -Y, \tau, X, \tau, -Y, \tau, -Y, \tau, X, \tau$
$-Y, \tau, X, \tau, X, \tau, -Y, \tau, X, \tau, X, \tau$
$-Y, \tau, -X, \tau, -X, \tau, -Y, \tau, -X, \tau, -X, \tau$

## RL advantages and disadvantages

-   Generalized approach to learning problem: no assumed prior knowledge

-   Can tailor problem to specific system of interest (e.g. strongly
    coupled system, timing precision constraints)

-   Robustness against known errors by including them in simulation of
    spin system

```{=html}
<!-- -->
```
-   Computationally expensive

-   Poor accuracy of many-body spin simulations

-   No guarantees for convergence to optimal (or good) solution

## References
