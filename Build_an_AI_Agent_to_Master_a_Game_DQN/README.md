# Build an AI Agent to Master a Game (DQN)

A Deep Q-Network implementation in C++ using LibTorch — training an agent to play a simple game through epsilon-greedy exploration, experience replay, and Q-value bootstrapping.

---

## Problem Statement

How does an AI learn to play a game without being told the rules? DQN (Deep Q-Network) learns by trial and error: playing the game, receiving rewards, and updating a neural network to predict which actions lead to higher cumulative rewards. This project implements the full DQN loop in C++ for performance-critical RL.

---

## Approach + Why

| Decision | Choice | Why |
|----------|--------|-----|
| **Language** | C++ with LibTorch | RL training loops run millions of steps — C++ is 10-50x faster than Python for the inner loop |
| **Network** | 3-layer MLP (1→128→128→2) | Simple enough for a 1D state space; two outputs for two possible actions |
| **Exploration** | Epsilon-greedy (decay: 0.995) | Starts with 100% random actions, decays to 1% — explores early, exploits learned policy later |
| **Replay buffer** | Fixed-size (10,000) ring buffer | Stores (state, action, reward, next_state, done) tuples; breaks temporal correlation in training batches |
| **Target network** | Same network (no target net) | Simplified from original DQN — works for simple games but may cause instability in complex ones |
| **Training** | MSE loss on Q-value bootstrapping | `loss = (Q(s,a) - (r + γ·max_a' Q(s',a')))²` — the core DQN update rule |

---

## What I Learned

- **C++ RL training is 10-50x faster than Python.** The inner loop (action → step → store → sample → update) runs 500 episodes in seconds. Python's equivalent with PyTorch would take minutes due to GIL and interpreter overhead.
- **Epsilon decay rate is the key hyperparameter.** 0.995 means epsilon drops from 1.0 to 0.01 in ~900 episodes. Too fast (0.95) → insufficient exploration. Too slow (0.999) → wasted training on random actions.
- **Experience replay is essential.** Without it, consecutive samples are highly correlated (same game state), causing the network to oscillate. The replay buffer decorrelates training samples.
- **LibTorch's C++ API is verbose but complete.** Every PyTorch operation has a C++ equivalent — `torch::nn::Linear`, `torch::optim::Adam`, `torch::mse_loss`. The boilerplate is higher but the runtime is worth it.

---

## Key Insight

> "RL agents don't learn from experience — they learn from the *difference* between what they expected and what happened. The Q-value update is literally `error = prediction - reality`, and that error is the teacher."

---

## Running

```bash
# Requires CMake and LibTorch
mkdir build && cd build
cmake .. && make
./dqn_agent
```
