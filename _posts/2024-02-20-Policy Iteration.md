---
title: "[Study Notes] Policy Iteration in RL"
date: 2024-02-21
categories:
  - Study Notes
tags:
  - reinforcement learning
  - RL
  - algorithms
toc_sticky: true
---

Policy Iteration is an algorithm that iteratively evaluates and improves a policy until it converges to the optimal policy.

**Steps of Policy Iteration**:
1. **Initialization**:
   - Start with an arbitrary policy $$\pi_0$$.

2. **Policy Evaluation**:
   - Evaluate the current policy $$\pi_k$$ by computing the state-value function $$V^{\pi_k}$$. This involves solving the system of linear equations:
    $$V^{\pi_k}(s) = \sum_{a \in \mathcal{A}(s)} \pi_k(a|s) \sum_{s' \in \mathcal{S}} P(s'|s, a) \left[ R(s, a, s') + \gamma V^{\pi_k}(s') \right]$$

3. **Policy Improvement**:
   - Improve the policy by acting greedily with respect to the current value function $$V^{\pi_k}$$ to obtain a new policy $$\pi_{k+1}$$:
    $$\pi_{k+1}(s) = \arg\max_{a \in \mathcal{A}(s)} \sum_{s' \in \mathcal{S}} P(s'|s, a) \left[ R(s, a, s') + \gamma V^{\pi_k}(s') \right]$$

4. **Convergence Check**:
   - If the policy does not change, i.e., $$\pi_{k+1} = \pi_k$$, then the algorithm has converged and $$\pi_k$$ is the optimal policy $$\pi^*$$.
   - Otherwise, set $$k = k + 1$$ and repeat the policy evaluation and improvement steps.

**Pros**:
- **Efficient Convergence**: Often converges faster than Value Iteration because it directly improves the policy in each iteration.
- **Policy Evaluation Step**: Allows for an exact computation of the value function for the current policy.

**Cons**:
- **Policy Evaluation Complexity**: The policy evaluation step can be computationally expensive, especially for large state spaces, as it requires solving a system of linear equations.
- **Implementation Complexity**: Requires careful implementation to ensure accurate policy evaluation and improvement steps.


| Feature                | Value Iteration                         | Policy Iteration                          |
| ---------------------- | --------------------------------------- | ----------------------------------------- |
| **Approach**           | Iteratively updates value function      | Iteratively evaluates and improves policy |
| **Convergence**        | Guaranteed to converge to $$V^*$$        | Guaranteed to converge to $$\pi^*$$       |
| **Speed**              | May require many iterations to converge | Typically faster convergence              |
| **Computational Cost** | High due to frequent value updates      | High due to policy evaluation step        |
| **Implementation**     | Simpler and more direct                 | More complex due to policy evaluation     |


#### **References**:
- Sutton, R. S., & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*. MIT Press.