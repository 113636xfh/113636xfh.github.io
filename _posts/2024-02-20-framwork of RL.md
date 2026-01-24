---
title: "[Study Notes] Framwork of RL"
date: 2024-02-22
categories:
  - Study Notes
tags:
  - reinforcement learning
  - RL
  - algorithms
toc_sticky: true
---


The key ider behind the Reinforcement Learning is that an agent will learn a policy $$a = \pi(s)$$ from the enviroment by interacting with it (throght trial and error) and recieve rewards (positive and negative) as a feedback for improve actions.

formal definition:

Reinforcement learning is a framework for solving control tasks (also called decision problems) by building agents that learn from the environment by interacting with it through trial and error and receiving rewards (positive or negative) as unique feedback.

# Terminology

## Observations /States Space
The Observations /States Space is the all possible Observation/states in the enviroment.
## Action Space
The Action Space is the all possible actions in the enviroment.

## Reward and Discounting
The reward is fundamental in RL because it' s the only feedback for the agent. Thanks to that, we can learn if a action is good or bad.

The in somewhat scenario, agent may face that how to decide between 2 choices that get a reeard right now and get another reward after a long time which may bigger. How we tradeoff 2 choice? It introduces the discounting. Discounting is a number in $$[0, 1]$$ that respresent how much a reward in the future should be discounted to compare the reward currently.




## Markov decision process (MDP)

The finite Markov Decision Process is illustrated in the figure below.
![Markov Decision Process Framework](/images/image.png)

In [probability theory](https://en.wikipedia.org/wiki/Probability_theory "Probability theory") and [statistics](https://en.wikipedia.org/wiki/Statistics "Statistics"), the term Markov property refers to the [memoryless](https://en.wikipedia.org/wiki/Memoryless "Memoryless") property of a [stochastic process](https://en.wikipedia.org/wiki/Stochastic_process "Stochastic process"), which means that its **future evolution is independent of its history.**

### Markov property

The Markov property states that the conditional probability of a state at time $$t+1$$, given all the states up to time $$t$$, is independent of all states prior to time $$t$$ and only depends on the state at time $$t$$.
$$\displaystyle P(X_{n+1}=x_{n+1}\mid X_{n}=x_{n},\dots ,X_{1}=x_{1})=P(X_{n+1}=x_{n+1}\mid X_{n}=x_{n}){\text{ for all }}n\in \mathbb {N}  
$$

## V and Q value

State value function (V function):

$$v_{\pi}(s) = E_{\pi}[G_{t}|S_{t} = s]$$

State-action value function (Q function):

$$q_{\pi}(s, a) = E_{\pi}[G_{t}|S_{t}=s, A_{t} = a]$$

$$v_\pi$$ and $$q_\pi$$ are related as follows:

$$v_{\pi}(s) = \sum\limits_{a\in \mathcal {A}(s)}\pi(a|s)q_{\pi}(s, a)$$


## Bellman Equations

The Bellman Equations establish a connection between the $$v_\pi$$ or $$q_{\pi}$$ of different states.

$$v_\pi(s) = E_\pi[R_{t+1}+\gamma v_\pi(s_{t+1})|s_t =s]$$

$$q_\pi(s, a) = E_\pi[R_{t+1} + \gamma q_\pi(s_{t+1}, a_{t+1})|S_t = s, a_t = a]$$

Expanding the expectation, we can obtain:

$$q_{\pi}(s, a) = \sum\limits_{\begin{array}ss'\in \mathcal{S}\\ r\in \mathcal{R}\end{array}}p(s', r|s, a)[r+\gamma v_{\pi}(s')]$$

Substituting the relation expression of $$v$$ and $$q$$ into the expression above, we can get the Bellman equation for $$v$$.

## Notations
![RL Notations](/images/image-1.png)

# Assumption
Some major assumptions are needed, as indicated in the figure below.

![MDP Assumptions](/images/image-2.png)

# Method

## Categories
### Policy-base Vs Value-base

In Policy-base method we learn a policy function directly that maximizes the expectation of cumulative rewards.
In Value-base method we learn a value function that maps a state to the expected value of being at this state. The value of this state is expected cumulative discounted reward the agent can get if starts in this state, and then according to our policy.

### Monte Carlo vs Temporal Difference Learning
Monte Carlo and Temporal Difference learning are 2 different strategies on how to train our value function or our policy function.
On one hand, Monte Carlo uses **an entire episode of experience before learning.** On the other hand, Temporal Difference uses **only a step** $$(S_{t},A_t,R_{t+1},S_{t+1})$$ **to learn.**

The TD error is written compactly as:
$$
\delta_t = G_t^{(1)} - V(S_t) = R_{t+1} + \gamma V(S_{t+1}) - V(S_t)
$$
We use the TD error to update the current state value so that $$V(S_t)$$ moves closer to the TD target.

### Off-policy vs On-policy
On-policy methods learn the value of the same policy that generates experience, while off-policy methods learn the value of a different target policy using behavior generated elsewhere.

- **Behavior policy ($$\mu$$)**: The policy used to interact with the environment and collect trajectories $$s \rightarrow a \rightarrow r \rightarrow s'$$.
- **Target policy ($$\pi$$)**: The policy we ultimately want to optimize.

**On-policy ($$\mu \equiv \pi$$)**
- Data generation and learning are synchronized; each update uses the latest policy.
- Transitions must include $$(s, a, r, s', a')$$ because the next action $$a'$$ also follows the current policy.
- Old data become stale after a policy update, so reuse is limited.

**Off-policy ($$\mu \neq \pi$$)**
- Behavior and target policies differ; data can come from older policies or other agents.
- Updates typically use $$(s, a, r, s')$$; the target action is computed from $$\pi$$ during learning.
- Enables replay buffers and broad data reuse but requires corrections (e.g., importance sampling) to control bias.


Examples and buckets:
- Q-learning — value-based, off-policy TD control.
- SARSA — value-based, on-policy TD control.


## classical RL method

### Value Iteration vs. Policy Iteration

Value-based view: **iteratively solve a linear system** where the unknown is $$V$$.
These methods assume a known environment model: explicit transition probabilities $$P(s'\mid s,a)$$ and reward function $$R(s,a,s')$$, i.e., model-based reinforcement learning.
### Policy Iteration
#### Core idea
"Evaluate, then improve, repeat until the policy stops changing." Policy Iteration alternates **policy evaluation** and **policy improvement** until convergence to the optimal policy.

#### Algorithm steps
1. **Initialize**
   - Randomly initialize a policy $$\pi_0$$ (e.g., pick a random action $$a$$ for each state $$s$$).
   - Initialize the state-value function $$V_0(s)=0,\ \forall s \in S$$.

2. **Policy Evaluation**
   Solve the Bellman expectation equation for the current policy $$\pi_k$$ to get an accurate value function $$V^{\pi_k}$$:
   $$V^{\pi_k}(s) = \sum_{a}\pi_k(a|s) \left[ R(s,a) + \gamma \sum_{s'}P(s'|s,a)V^{\pi_k}(s') \right]$$

3. **Policy Improvement**
   Using $$V^{\pi_k}$$, update the policy greedily to obtain $$\pi_{k+1}$$:
   $$\pi_{k+1}(a|s) = \begin{cases} 1 & \text{if } a = \arg\max_{a'} \left[ R(s,a') + \gamma \sum_{s'}P(s'|s,a')V^{\pi_k}(s') \right] \\ 0 & \text{otherwise} \end{cases}$$
   Intuition: choose, for each state, the action that maximizes immediate reward plus discounted future value.

4. **Convergence check**
   If $$\pi_{k+1} = \pi_k$$, stop—the policy is optimal $$\pi^*$$. Otherwise, return to step 2.


### Value Iteration
#### Core idea
"Iterate the value function directly; policy improvement is implicit." Value Iteration repeatedly applies the **Bellman optimality equation** so $$V$$ converges to $$V^*$$, then extracts $$\pi^*$$ greedily.

In essence, Value Iteration is a **simplified Policy Iteration**—it merges "one-step policy evaluation + policy improvement" into a single update.

#### Algorithm steps
1. **Initialize**
   Set $$V_0(s)=0,\ \forall s \in S$$.

2. **Value updates**
   For each state $$s$$, update via the Bellman optimality equation:
   $$V_{k+1}(s) = \max_{a} \left[ R(s,a) + \gamma \sum_{s'}P(s'|s,a)V_k(s') \right]$$
   This simultaneously performs **approximate policy evaluation** and **greedy improvement**.

3. **Convergence check**
   Stop when $$\max_s |V_{k+1}(s)-V_k(s)| < \theta$$; then $$V_k$$ has converged to $$V^*$$.

4. **Extract optimal policy**
   Derive $$\pi^*$$ greedily from $$V^*$$:
   $$\pi^*(a|s) = \begin{cases} 1 & \text{if } a = \arg\max_{a'} \left[ R(s,a') + \gamma \sum_{s'}P(s'|s,a')V^*(s') \right] \\ 0 & \text{otherwise} \end{cases}$$


### Key Differences

| Dimension               | Policy Iteration                        | Value Iteration                         |
|-------------------------|-----------------------------------------|-----------------------------------------|
| **Object**              | Explicit policy $$\pi$$; alternate eval + improve | Implicit policy; iterate $$V$$ only        |
| **Evaluation step**     | Solve Bellman expectation exactly for $$V^\pi$$ | One-step Bellman optimality update (approx $$V$$) |
| **Loop logic**          | Policy eval → policy improve → repeat   | Value update (eval+improve) → repeat → extract policy |
| **Convergence**         | Fewer iterations; each more expensive   | More iterations; cheaper per update      |
| **Compute cost**        | Higher (solve linear system)            | Lower (no linear system solve)           |
| **Use cases**           | Small state spaces; need fast convergence | Large state spaces; need efficiency      |
| **Output**              | Optimal $$\pi^*$$ plus $$V^*$$              | Optimal $$V^*$$ plus $$\pi^*$$ derived from it |


## Modern Method
Core idea: use neural networks for function approximation.

### Q-learning and Deep Q-learning

**Deep Q-learning (DQN)** extends Q-learning by using deep neural networks to approximate the Q-function, enabling learning in high-dimensional state spaces.

### Policy Gradient Method

Policy Gradient methods directly optimize the policy by computing gradients of expected reward with respect to policy parameters. Unlike value-based methods, they can handle continuous action spaces and stochastic policies naturally.

### Actor Critic (A2C)
Combines value-based and policy-based methods.
- _An Actor_ that controls **how our agent behaves** (Policy-Based method)
- _A Critic_ that measures **how good the taken action is** (Value-Based method)



# Prediction Vs Control

A prediction task in RL is where the policy is supplied, and the goal is to measure how well it performs. That is, to predict the expected total reward from any given state assuming the function $$\pi(a \mid s)$$ is fixed.

A control task in RL is where the policy is not fixed, and the goal is to find the optimal policy. That is, to find the policy $$\pi(a \mid s)$$ that maximises the expected total reward from any given state.



# Source
[A CSDN blog about RL fundament](https://blog.csdn.net/weixin_46185085/article/details/107598554)

[Youtube video tutorial about RL fundament](https://www.youtube.com/watch?v=_j6pvGEchWU&list=PLzvYlJMoZ02Dxtwe-MmH4nOB5jYlMGBjr&index=2)

