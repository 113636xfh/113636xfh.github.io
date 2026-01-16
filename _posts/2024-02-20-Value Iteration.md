---
title: "[Study Notes] Value Iteration in RL"
date: 2024-02-22
categories:
  - Study Notes
tags:
  - reinforcement learning
  - RL
  - algorithms
toc_sticky: true
---


Value Iteration is an iterative algorithm that combines policy evaluation and policy improvement in a single stValueep. It iteratively updates the value function until it converges to the optimal value function, from which the optimal policy can be derived.

**Steps of Value Iteration**:
1. **Initialization**:
   - Start with an arbitrary value function $V_0$.

2. **Value Update**:
   - Iteratively update the value function using the Bellman optimality equation:
    $$V_{k+1}(s) = \max_{a \in \mathcal{A}} \sum_{s' \in \mathcal{S}} P(s'|s, a) \left[ R(s, a, s') + \gamma V_k(s') \right]$$

3. **Convergence Check**:
   - Repeat the value update step until the value function converges, i.e., the change in value function is below a small threshold.

4. **Derive Policy**:
   - Once the value function has converged, derive the optimal policy:
    $$\pi^*(s) = \arg\max_{a \in \mathcal{A}} \sum_{s' \in \mathcal{S}} P(s'|s, a) \left[ R(s, a, s') + \gamma V^*(s') \right]$$

**Pros**:
- **Simple and Direct**: Combines policy evaluation and improvement in a single step.
- **Guaranteed Convergence**: Guaranteed to converge to the optimal value function$V^*$.

**Cons**:
- **Computationally Intensive**: Can be computationally expensive due to the need to update the value function for all states in each iteration.
- **Slow Convergence**: May require many iterations to converge, especially for large state spaces.



## Value Iteration Proof
#### Theoretical Background

To prove the convergence of Value Iteration, we use the contraction mapping theorem. The Bellman optimality operator is a contraction mapping with respect to the sup norm (also known as the infinity norm).

A function $$T$$ is a contraction mapping if there exists a constant $$0 \leq \gamma < 1$$ such that for any two functions $$V$$ and $$V'$$:
$$\| T(V) - T(V') \|_{\infty} \leq \gamma \| V - V' \|_{\infty}$$
where $$\| \cdot \|_{\infty}$$ denotes the sup norm:
$$\| V \|_{\infty} = \max_{s \in \mathcal{S}} |V(s)|$$
#### Proof

1. **Bellman Optimality Operator**:
   Consider the Bellman optimality operator $$T$$:
  $$T(V)(s) = \max_{a \in \mathcal{A}} \sum_{s' \in \mathcal{S}} P(s'|s, a) \left[ R(s, a, s') + \gamma V(s') \right]$$

2. **Contraction Property**:
   Let $$V$$ and $$V'$$ be two arbitrary value functions. Then:
  $$|T(V)(s) - T(V')(s)| = \left| \max_{a} \sum_{s'} P(s'|s, a) [R(s, a, s') + \gamma V(s')] - \max_{a} \sum_{s'} P(s'|s, a) [R(s, a, s') + \gamma V'(s')] \right|$$

   Since the maximum of a set of values is always greater than or equal to the average value in the set, we can write:
  $$|T(V)(s) - T(V')(s)| \leq \max_{a} \left| \sum_{s'} P(s'|s, a) \gamma [V(s') - V'(s')] \right|$$

   Using the linearity of expectation and the fact that the transition probabilities $$P(s'|s, a)$$ sum to 1, we get:
  $$|T(V)(s) - T(V')(s)| \leq \gamma \sum_{s'} P(s'|s, a) |V(s') - V'(s')|$$

   By the definition of the sup norm:
  $$|T(V)(s) - T(V')(s)| \leq \gamma \| V - V' \|_{\infty}$$

   Since this holds for all states $$s$$, we have:
  $$\| T(V) - T(V') \|_{\infty} \leq \gamma \| V - V' \|_{\infty}$$

   This proves that $$T$$ is a contraction mapping with contraction coefficient $$\gamma$$.

3. **Contraction Mapping Theorem**:
   By the Banach fixed-point theorem (also known as the contraction mapping theorem), a contraction mapping on a complete metric space has a unique fixed point. In our case, this fixed point is the optimal value function $$V^*$$.

   Therefore, iteratively applying $$T$$ to any initial value function $$V_0$$ will converge to $$V^*$$:
  $$V_k \to V^* \text{ as } k \to \infty$$

#### Conclusion

The Value Iteration algorithm is guaranteed to converge to the optimal value function $$V^*$$ due to the contraction mapping property of the Bellman optimality operator. This convergence proof ensures that Value Iteration is a reliable method for solving Markov Decision Processes with finite state and action spaces and a discount factor $$\gamma$$ in the range $$0 \leq \gamma < 1$$.


#### **References**:
- Sutton, R. S., & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*. MIT Press.