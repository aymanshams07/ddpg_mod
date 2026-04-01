# 🚀 Deep Deterministic Policy Gradient (DDPG) for Continuous Control

---

## 📌 Overview

This project explores the application of **Deep Reinforcement Learning (DRL)** using the **Deep Deterministic Policy Gradient (DDPG)** algorithm to solve continuous control tasks.

We evaluate performance on three OpenAI Gym environments:
- 🌙 `LunarLanderContinuous-v2`
- 🚶 `BipedalWalker-v3`
- 🧗 `BipedalWalkerHardcore-v3`

We also introduce a modified version:
> ⚙️ **DDPG-MOD** = Improved policy network + Gaussian exploration

---

## 🎯 Motivation

Traditional RL methods like **Q-learning** work well in **discrete spaces**, but struggle with:

- 🔄 Continuous action spaces (infinite actions)
- 📊 High-dimensional states
- ⚠️ Training instability due to correlated data

💡 **Solution: Deep Reinforcement Learning**
- Learns features end-to-end
- Handles complex environments

⚠️ **Challenge**: Sequential data is highly correlated → unstable training

✔️ **Fix: Experience Replay**
- Stores transitions: `(s, a, r, s')`
- Random sampling → breaks correlation → stabilizes training

---

## 🧠 Methodology: DDPG

DDPG is an **off-policy actor–critic algorithm** for continuous control.

### 🔹 Components

- 🎭 **Actor Network**  
  Learns policy:  
  $$ a = \mu(s) $$

- 🧮 **Critic Network**  
  Learns value function:  
  $$ Q(s, a) $$

---

### ⚙️ Key Mechanisms

#### 📦 Replay Buffer
- Stores past experiences `(s, a, r, s')`
- Enables random batch training

#### 🎯 Target Networks
Stabilize learning using Polyak averaging:
$$
\theta_{\text{target}} \leftarrow \rho \, \theta_{\text{target}} + (1 - \rho)\, \theta
$$

#### 🔁 Bellman Update
$$
Q(s,a) = r + \gamma \, Q'(s', \mu'(s'))
$$

#### 📈 Policy Optimization
$$
\max_{\theta} \, \mathbb{E}_{s \sim D} \left[ Q(s, \mu_{\theta}(s)) \right]
$$

---

## 🎲 Exploration Strategy

Since DDPG is deterministic, we add noise for exploration:

- 🌪️ **Ornstein–Uhlenbeck Noise**
  - Temporally correlated
  - Used in original DDPG

- 🎯 **Gaussian Noise (DDPG-MOD)**
  - Simpler and more effective exploration

📉 Noise is gradually reduced → better convergence

---

## 🌍 Environments

### 🌙 LunarLanderContinuous-v2
- Objective: land at **(0,0)**
- Continuous thrust control
- 🎯 Target reward: ≥ 200

---

### 🚶 BipedalWalker-v3
- Learn stable walking
- 🏁 +300 for completion  
- 💥 −100 for falling

---

### 🧗 BipedalWalkerHardcore-v3
- Rough terrain + obstacles
- Requires strong coordination and balance

---

## 🏗️ Network Architecture

- 🧠 Actor & Critic:
  - 2 hidden layers: **400, 300 units**
- 🔚 Actor output: `tanh` (bounded actions)
- ⚡ Optimizer: Adam

---

### ⚙️ Hyperparameters

| Parameter            | Value      |
|---------------------|-----------|
| Replay Buffer       | 1e4       |
| Discount Factor γ   | 0.99      |
| Actor LR            | 1e-4      |
| Critic LR           | 1e-3      |
| Target Update (ρ)   | 0.01      |

---

## 🏋️ Training Setup

- 🔁 2000 episodes per environment (both models)
- ⏳ Extended training:
  - DDPG-MOD → **6000 episodes**
- 📊 Evaluation:
  - Over **50,000 benchmark episodes**

---

## 📊 Results

### 🏆 Overall Performance

DDPG-MOD outperformed classical DDPG:

- 🚀 **+54.5%** → BipedalWalkerHardcore  
- 🚀 **+24.4%** → LunarLanderContinuous  

---

### 🌙 LunarLander Insights

- Both agents learned stable landing

🔴 **DDPG**
- Learned **energy-saving free-fall**
- Caused instability (reward spikes)

🟢 **DDPG-MOD**
- Better exploration (Gaussian noise)
- Achieved **perfect landings (>200 reward)**

💡 Insight: Exploration strategy affects precision and stability

---

### 🚶 BipedalWalker Insights

- Much harder due to locomotion + balance

🔴 **DDPG**
- Learned **stationary behavior** (no movement)

🟢 **DDPG-MOD**
- Better **left–right coordination**
- Improved **pelvis stability**
- Faster task completion

💡 Insight: High reward ≠ correct behavior

---

## 🔍 Key Observations

- 🎯 Exploration strongly impacts performance
- ⚠️ Reward can be misleading → must analyze behavior
- 🤖 Emergent behaviors observed:
  - Landing zone exploitation
  - Stationary stability strategy

---

## 🧪 Contributions

- ⚙️ Developed **DDPG-MOD**
- 📊 Compared exploration strategies
- 🧠 Studied policy architecture impact
- 🌍 Unified benchmark across 3 environments

---

## ⚠️ Limitations

- Did not fully converge in all environments
- Sensitive to:
  - Hyperparameters
  - Exploration noise
- Sample inefficiency

---

## 🔮 Future Work

- 🚀 TD3 (Twin Delayed DDPG)
- 🔥 SAC (Soft Actor-Critic)
- 🧭 Better exploration strategies
- 📈 Improved sample efficiency
- 🎓 Curriculum learning

---

## 📚 References

- Lillicrap et al., *Continuous Control with Deep Reinforcement Learning*
- OpenAI Gym
- Related works on BipedalWalker & LunarLander

---

## ▶️ How to Run

```bash
git clone <repo_url>
cd ddpg-continuous-control
pip install -r requirements.txt
python train.py --env LunarLanderContinuous-v2
