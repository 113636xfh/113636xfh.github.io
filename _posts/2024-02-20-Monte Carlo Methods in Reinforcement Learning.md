---
title: "[Study Notes] Monte Carlo Methods in RL"
date: 2024-02-22
categories:
  - Study Notes
tags:
  - reinforcement learning
  - RL
  - algorithms
toc_sticky: true
---


Monte Carlo (MC) methods are a class of algorithms used in reinforcement learning to estimate the value of states or state-action pairs based on sample episodes. Unlike [[dynamic programming methods]], which require a complete model of the environment, Monte Carlo methods can learn directly from raw experience without requiring knowledge of the environment's dynamics.
![[Pasted image 20241024215710.png]]
#### Key Concepts

1. **Episodes**:
   - Monte Carlo methods require complete episodes (sequences of states, actions, and rewards) from the beginning to the terminal state.
   - An episode is a sequence$S_0, A_0, R_1, S_1, A_1, R_2, \ldots, S_T$, where $T$ is the terminal state.

3. **Value Estimation**:
   - Monte Carlo methods estimate the value of a state or state-action pair by averaging the returns observed after visiting that state or state-action pair.
# Steps

#### Monte Carlo Prediction

Monte Carlo prediction is used to estimate the value function $V(s)$ for a **given policy** $\pi$. The steps are as follows:

Monte Carlo methods estimate the value of states or state-action pairs based on sample episodes. Here are the steps involved in Monte Carlo prediction and control:

#### 1. Initialization
- Initialize the value function $V(s)$ for each state $s$ arbitrarily (or to zero).
- Alternatively, initialize the action-value function $Q(s, a)$ for each state-action pair$(s, a)$arbitrarily.
- Initialize the policy$\pi$ (it could be a random policy, an exploration policy, or some other predefined policy).
- Initialize a counter or array to keep track of the number of times each state (or state-action pair) has been visited.

#### 2. Generate Episodes
- **Episode**: A sequence of states, actions, and rewards, from the start state to a terminal state.
- **Generate an Episode**: Using the current policy $\pi$, generate an episode by interacting with the environment until a terminal state is reached.
  - Start from an initial state $s_0$.
  - Choose an action $a_t$ based on the policy $\pi(s_t)$.
  - Execute the action$a_t$to observe the reward$r_{t+1}$and next state$s_{t+1}$.
  - Repeat until reaching a terminal state.

#### 3. Calculate Returns
- **Return**: The total accumulated reward from time step $t$ onwards.
  - If the episode is$s_0, a_0, r_1, s_1, a_1, r_2, ..., s_T$, where $T$ is the final time step, then the return$G_t$from time step $t$ is given by:
   $$
    G_t = r_{t+1} + \gamma r_{t+2} + \gamma^2 r_{t+3} + \ldots + \gamma^{T-t} r_{T}
   $$
  - Here,$\gamma$ is the discount factor ($0 \leq \gamma \leq 1$).

#### 4. Update Value Function
- **For State Values$V(s)$**:
  - For each state $s$ appearing in the episode, calculate the average return following all the visits to $s$.
  - If using the first-visit method, update the value of the state $s$ using only the first occurrence of $s$ in each episode.
  - If using the every-visit method, update the value of the state $s$ using every occurrence of $s$ in each episode.
  - Update rule for first-visit or every-visit Monte Carlo:
   $$
    V(s) \leftarrow V(s) + \alpha (G_t - V(s))
   $$
  - Here, $\alpha$ is the step-size parameter, and $G_t$ is the return observed from the episode.

- **For Action Values $Q(s, a)$**:
  - For each state-action pair $(s, a)$ appearing in the episode, calculate the average return following all the visits to$(s, a)$.
  - Update rule for first-visit or every-visit Monte Carlo:
   $$
    Q(s, a) \leftarrow Q(s, a) + \alpha (G_t - Q(s, a))
   $$
  - Use$Q(s, a)$values to improve the policy.

#### 5. Policy Improvement
- **Policy Improvement**: Update the policy$\pi$based on the updated value function or action-value function.
  - For state values$V(s)$:
   $$
    \pi(s) = \arg\max_a \sum_{s'} P(s'|s,a) [R(s, a, s') + \gamma V(s')]
   $$
  - For action values$Q(s, a)$:
   $$
    \pi(s) = \arg\max_a Q(s, a)
   $$
  - The policy is improved greedily based on the updated values.

#### 6. Repeat
- Repeat steps 2 to 5 for a large number of episodes to ensure convergence of the value function and the policy.


#### Monte Carlo Control

Monte Carlo control aims to find the optimal policy by learning both the value function and the policy. The steps are as follows:

1. **Initialization**:
   - Initialize the action-value function$Q(s, a)$arbitrarily for all state-action pairs$(s, a) \in \mathcal{S} \times \mathcal{A}$(e.g.,$Q(s, a) = 0$for all$(s, a)$).
   - Initialize an empty list for each state-action pair to store returns.
   - Initialize a policy$\pi$(e.g., a random policy).

2. **Generate Episodes**:
   - Generate an episode using the policy$\pi$. An episode is a sequence of states, actions, and rewards:$(S_0, A_0, R_1, S_1, A_1, R_2, \ldots, S_T)$, where$T$is the terminal state.

3. **Calculate Returns**:
   - For each state-action pair$(s, a)$in the episode, calculate the return$G_t$from that state-action pair:
    $$G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \ldots + \gamma^{T-t-1} R_T$$

4. **Update Action-Value Function**:
   - Append the return$G_t$to the list of returns for the state-action pair$(s, a)$.
   - Update the action-value function$Q(s, a)$to be the average of the returns:
    $$Q(s, a) = \frac{\sum \text{Returns}(s, a)}{\text{Number of Returns}(s, a)}$$

5. **Policy Improvement**:
   - Improve the policy$\pi$by acting greedily with respect to the current action-value function$Q(s, a)$:
    $$\pi(s) = \arg\max_{a \in \mathcal{A}} Q(s, a)$$

6. **Repeat**:
   - Repeat steps 2-5 for many episodes until the policy$\pi$converges to the optimal policy$\pi^*$.
#### Types of Monte Carlo Methods

1. **First-Visit Monte Carlo**:
   - Estimates the value of a state as the average of the returns following the first visit to that state in an episode.

2. **Every-Visit Monte Carlo**:
   - Estimates the value of a state as the average of the returns following every visit to that state in an episode.

#### Advantages and Disadvantages

**Advantages**:
- **Model-Free**: Does not require knowledge of the environment's dynamics.
- **Simple**: Conceptually straightforward and easy to implement.

**Disadvantages**:
- **Sample Inefficiency**: Requires many episodes to get accurate estimates, especially for large state spaces.
- **Requires Episodic Tasks**: Assumes episodes terminate, making it less suitable for continuous tasks without modifications.

#### Summary

Monte Carlo methods are a powerful tool in reinforcement learning, particularly useful for tasks where the environment's model is unknown and direct interaction is possible. They provide a foundation for more advanced methods, such as [[Temporal-Difference learning|Temporal-Difference (TD) learning]] and reinforcement learning algorithms used in practice.

---

**References**:
- Sutton, R. S., & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*. MIT Press.
### Steps of Monte Carlo Methods in Reinforcement Learning






### Example Pseudocode for Monte Carlo Control (Every-Visit):

```python
Initialize Q(s, a) arbitrarily for all s, a
Initialize policy π to be epsilon-greedy w.r.t. Q
For each episode:
    Initialize: S ← initial state
    Generate episode following π: S0, A0, R1, S1, A1, R2, ..., ST
    For each state-action pair (S, A) in the episode:
        G ← return following the first occurrence of (S, A)
        N(S, A) ← N(S, A) + 1  # Increment visit counter
        Q(S, A) ← Q(S, A) + (1 / N(S, A)) * (G - Q(S, A))  # Update action-value function
    For each state S in the episode:
        π(S) ← argmax_a Q(S, a)  # Update policy to be greedy w.r.t. Q
```


### Advantages and Disadvantages

**Advantages**:
- **Model-Free**: Does not require knowledge of the environment's dynamics.
- **Simple**: Conceptually straightforward and easy to implement.

**Disadvantages**:
- **Sample Inefficiency**: Requires many episodes to get accurate estimates, especially for large state spaces.
- **Requires Episodic Tasks**: Assumes episodes terminate, making it less suitable for continuous tasks without modifications.

#### Summary

Monte Carlo methods are a powerful tool in reinforcement learning, particularly useful for tasks where the environment's model is unknown and direct interaction is possible. They provide a foundation for more advanced methods, such as Temporal-Difference (TD) learning and reinforcement learning algorithms used in practice.

---

**References**:
- Sutton, R. S., & Barto, A. G. (2018). *Reinforcement Learning: An Introduction*. MIT Press.