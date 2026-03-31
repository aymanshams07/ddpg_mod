# Deep Deterministic Policy Gradient (DDPG) for Continuous Control

## Overview

This project explores the application of **Deep Reinforcement Learning (DRL)**, specifically the **Deep Deterministic Policy Gradient (DDPG)** algorithm, for solving continuous control problems.

We evaluate performance on three OpenAI Gym environments:
- `LunarLanderContinuous-v2`
- `BipedalWalker-v3`
- `BipedalWalkerHardcore-v3`

In addition to the standard DDPG implementation, we propose a modified version (**DDPG-MOD**) with:
- A modified actor (policy) network
- Alternative exploration strategy using Gaussian noise

---

## Motivation

Traditional reinforcement learning methods such as Q-learning are effective in **discrete state-action spaces**, but fail in **continuous domains** due to:

- Infinite action spaces
- High-dimensional observations
- Training instability due to correlated data

DDPG addresses these challenges using:
- Actor–Critic architecture
- Experience Replay
- Target Networks

This project investigates:
- How well DDPG performs in continuous environments
- How exploration strategy and policy design affect performance

---

## Methodology

### Deep Deterministic Policy Gradient (DDPG)

DDPG is an **off-policy actor–critic algorithm** designed for continuous action spaces.

- **Actor Network**: learns policy \( a = \mu(s) \)
- **Critic Network**: learns Q-function \( Q(s, a) \)

#### Key Components

- **Replay Buffer**
  - Stores transitions \( (s, a, r, s') \)
  - Breaks temporal correlations via random sampling

- **Target Networks**
  - Stabilize training using Polyak averaging:

θ_target ← ρ θ_target + (1 - ρ) θ


- **Bellman Update**

Q(s,a) = r + γ Q'(s', μ'(s'))


- **Policy Optimization**

max E[ Q(s, μ(s)) ]


---

## Exploration Strategy

Since DDPG uses a deterministic policy, exploration is introduced by adding noise to actions:

- **Ornstein–Uhlenbeck (OU) Noise**
- Temporally correlated noise (used in original DDPG)

- **Gaussian Noise (DDPG-MOD)**
- Simpler and promotes broader exploration

Noise is reduced over time to allow convergence.

---

## Environments

### 1. LunarLanderContinuous-v2
- Objective: Land safely at position (0,0)
- Continuous thrust control
- Reward target: ≥ 200

### 2. BipedalWalker-v3
- Objective: Learn stable locomotion
- Reward: +300 for completion, −100 for falling

### 3. BipedalWalkerHardcore-v3
- More difficult terrain:
- Obstacles (ladders, stumps, pitfalls)
- Requires robust coordination and balance

---

## Network Architecture

- Actor & Critic Networks:
- 2 Hidden Layers: **400, 300 units**
- Activation:
- Final actor layer uses **tanh**
- Optimizer:
- Adam

### Hyperparameters

| Parameter            | Value        |
|---------------------|-------------|
| Replay Buffer Size  | 1e4         |
| Discount Factor γ   | 0.99        |
| Actor LR            | 1e-4        |
| Critic LR           | 1e-3        |
| Target Update Rate  | 0.01        |

---

## Training Setup

- Each model trained for:
- **2000 episodes per environment**
- Extended training:
- **DDPG-MOD trained for 6000 episodes**
- Evaluation:
- Over **50,000 benchmark episodes**

---

## Results

### Overall Performance

DDPG-MOD outperformed classical DDPG:

- **+54.5% improvement** in BipedalWalkerHardcore
- **+24.4% improvement** in LunarLanderContinuous

---

### LunarLander Insights

- Both agents learned stable landing behavior
- Classical DDPG:
- Learned **energy-saving free-fall strategy**
- Caused instability in later episodes
- DDPG-MOD:
- Better exploration using Gaussian noise
- Achieved **perfect landings (>200 reward)**

---

### BipedalWalker Insights

- Learning locomotion is significantly harder
- Classical DDPG:
- Sometimes learned **stationary behavior**
- DDPG-MOD:
- Better **leg coordination**
- Improved **balance and locomotion**
- Completed tasks faster

---

## Key Observations

- Exploration strategy strongly affects learning
- Reward alone can be misleading — behavior must be analyzed
- Emergent behaviors observed:
- **Landing zone exploitation** (gaming reward system)
- **Stationary stability** (avoiding movement)

---

## Contributions

- Comparative study of **DDPG vs DDPG-MOD**
- Demonstrated impact of:
- Policy architecture
- Exploration strategy
- Provided unified benchmark across:
- LunarLanderContinuous
- BipedalWalker
- BipedalWalkerHardcore

---

## Limitations

- Agent did not fully converge in all environments
- Sensitive to:
- Hyperparameters
- Exploration noise
- Sample inefficiency

---

## Future Work

- Apply more stable algorithms:
- **TD3 (Twin Delayed DDPG)**
- **SAC (Soft Actor-Critic)**
- Improve exploration strategies
- Increase sample efficiency
- Incorporate curriculum learning

---

## References

- Lillicrap et al., *Continuous Control with Deep Reinforcement Learning*
- OpenAI Gym Environments
- Additional benchmarking works on BipedalWalker and LunarLander

---

## How to Run

```bash
git clone <repo_url>
cd ddpg-continuous-control
pip install -r requirements.txt
python train.py --env LunarLanderContinuous-v2
Acknowledgements

This work was conducted as part of a Master's thesis in Computer Science, focusing on reinforcement learning and continuous control.
