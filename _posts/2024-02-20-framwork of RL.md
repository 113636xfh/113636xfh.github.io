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


The key ider behind the Reinforcement Learning is that an agent will learn from the enviroment by interacting with it (throght trial and error) and recieve rewards (positive and negative) as a feedback for improve actions.

formal definition:

Reinforcement learning is a framework for solving control tasks (also called decision problems) by building agents that learn from the environment by interacting with it through trial and error and receiving rewards (positive or negative) as unique feedback.

## Observations /States Space
The Observations /States Space is the all possible Observation/states in the enviroment.
## Action Space
The Action Space is the all possible actions in the enviroment.

## Reward and Discounting
The reward is fundamental in RL because it' s the only feedback for the agent. Thanks to that, we can learn if a action is good or bad.

The in somewhat scenario, agent may face that how to decide between 2 choices that get a reeard right now and get another reward after a long time which may bigger. How we tradeoff 2 choice? It introduces the discounting. Discounting is a number in $$[0, 1]$$ that respresent how much a reward in the future should be discounted to compare the reward currently.

# Introduction
The finite Markov Decision Process is illustrated in the figure below.
![Markov Decision Process Framework](/images/image.png)
## Notations
![RL Notations](/images/image-1.png)
# Fundamental Conception
## Markov decision process (MDP)

In [probability theory](https://en.wikipedia.org/wiki/Probability_theory "Probability theory") and [statistics](https://en.wikipedia.org/wiki/Statistics "Statistics"), the term Markov property refers to the [memoryless](https://en.wikipedia.org/wiki/Memoryless "Memoryless") property of a [stochastic process](https://en.wikipedia.org/wiki/Stochastic_process "Stochastic process"), which means that its **future evolution is independent of its history.**
## Markov property
The Markov property states that the conditional probability of a state at time $$t+1$$, given all the states up to time $$t$$, is independent of all states prior to time $$t$$ and only depends on the state at time $$t$$.
$$\displaystyle P(X_{n+1}=x_{n+1}\mid X_{n}=x_{n},\dots ,X_{1}=x_{1})=P(X_{n+1}=x_{n+1}\mid X_{n}=x_{n}){\text{ for all }}n\in \mathbb {N}  
$$
### Formalization

We want to create a policy that satisfies the Goal: $$\max\limits_{\pi}E_{\pi}[G_{t}]$$.

Aforementioned $$G_{t}$$ is the cumulative reward:

$$G_{t} = \sum\limits^T_{k = t+1}\gamma^{k-t-1}R_{k}$$

The policy $$\pi :{\mathcal {S}}\times {\mathcal {A}}\rightarrow [0,1]$$, where $$\pi (s, a) = Pr(A_t = a \mid S_t = s)$$, maximizes the expected cumulative reward.

We define two important value functions:

State value function:

$$v_{\pi}(s) = E_{\pi}[G_{t}|S_{t} = s]$$

State-action value function:

$$q_{\pi}(s, a) = E_{\pi}[G_{t}|S_{t}=s, A_{t} = a]$$

$$v_\pi$$ and $$q_\pi$$ are related as follows:

$$v_{\pi}(s) = \sum\limits_{a\in \mathcal {A}(s)}\pi(a|s)q_{\pi}(s, a)$$

Define the optimal policy:

$$v_{\pi^*}(s) \ge v_{\pi}(s)$$ for all $$s$$ and for any $$\pi$$

$$q_{\pi^*}(s, a) \ge q_{\pi}(s, a)$$ for all $$s,a$$ and for any $$\pi$$

And for optimal policy:

$$v_{*}(s) = \max\limits_{a\in {\mathcal A}(s)}q_{*}(s, a)$$
### Assumption
Some major assumptions are needed, as indicated in the figure below.

![MDP Assumptions](/images/image-2.png)

## Bellman Equations

The Bellman Equations establish a connection between the $$v_\pi$$ or $$q_{\pi}$$ of different states.

$$v_\pi(s) = E_\pi[R_{t+1}+\gamma v_\pi(s_{t+1})|s_t =s]$$

$$q_\pi(s, a) = E_\pi[R_{t+1} + \gamma q_\pi(s_{t+1}, a_{t+1})|S_t = s, a_t = a]$$

Expanding the expectation, we can obtain:

$$q_{\pi}(s, a) = \sum\limits_{\begin{array}ss'\in \mathcal{S}\\ r\in \mathcal{R}\end{array}}p(s', r|s, a)[r+\gamma v_{\pi}(s')]$$

Substituting the relation expression of $$v$$ and $$q$$ into the expression above, we can get the Bellman equation for $$v$$.


# Terminology
Episodes (sequences of states, actions, and rewards) 


# Method

## Policy-base Vs Value-base
There are 2 type of method to maximize its expected cumulative reward, Policy-base method and Value-base method.

In Policy-base method we learn a policy function directly that maximizes the expectation of cumulative rewards.
In Value-base method we learn a value function that maps a state to the expected value of being at this state. The value of this state is expected cumulative discounted reward the agent can get if starts in this state, and then according to our policy.
## Monte Carlo vs Temporal Difference Learning
Monte Carlo and Temporal Difference learning are 2 different strategies on how to train our value function or our policy function.
On one hand, Monte Carlo uses **an entire episode of experience before learning.** On the other hand, Temporal Difference uses **only a step** $$(S_{t},A_t,R_{t+1},S_{t+1})$$ **to learn.**

## Off-policy vs On-policy
In off-policy method we use a different policy for acting(inference) and updating(training).

Hybrid Method

**Advantage Actor Critic (A2C)**:
- _An Actor_ that controls **how our agent behaves** (Policy-Based method)
- _A Critic_ that measures **how good the taken action is** (Value-Based method)
## Value Iteration vs. Policy Iteration
Value Iteration and Policy Iteration are both algorithms used in reinforcement learning to find the optimal policy for a Markov Decision Process (MDP). Here’s a comparison of the two methods, along with their respective pros and cons.

| Feature                | Value Iteration                         | Policy Iteration                          |
| ---------------------- | --------------------------------------- | ----------------------------------------- |
| **Approach**           | Iteratively updates value function      | Iteratively evaluates and improves policy |
| **Convergence**        | Guaranteed to converge to $$V^*$$        | Guaranteed to converge to $$\pi^*$$       |
| **Speed**              | May require many iterations to converge | Typically faster convergence              |
| **Computational Cost** | High due to frequent value updates      | High due to policy evaluation step        |
| **Implementation**     | Simpler and more direct                 | More complex due to policy evaluation     |

Both algorithms have their own strengths and weaknesses, and the choice between them depends on the specific requirements of the problem, such as the size of the state space and the computational resources available.

## Monte Carlo Methods

Monte Carlo (MC) methods are a class of algorithms used in reinforcement learning to estimate the value of states or state-action pairs based on sample episodes. Unlike dynamic programming methods, which require a complete model of the environment, Monte Carlo methods can learn directly from raw experience without requiring knowledge of the environment's dynamics.
![Monte Carlo Illustration](/images/image-3.png)
## Temporal Difference Learning
![Temporal Difference Learning](/images/image-4.png)
Temporal Difference (TD) learning combines ideas from Monte Carlo methods and dynamic programming. TD methods can learn directly from raw experience without a model of the environment's dynamics, and they update estimates based on other learned estimates (bootstrapping).
## Function Approximation

## Q-learning and Deep Q-learning

**On-policy vs Off-policy Learning**: On-policy methods learn the value of the policy being used for control, while off-policy methods learn the value of a different policy from the one being used to generate behavior.

**Q-learning** is an off-policy TD control algorithm that directly approximates the optimal action-value function. **Deep Q-learning (DQN)** extends Q-learning by using deep neural networks to approximate the Q-function, enabling learning in high-dimensional state spaces.

## Policy Gradient Method

Policy Gradient methods directly optimize the policy by computing gradients of expected reward with respect to policy parameters. Unlike value-based methods, they can handle continuous action spaces and stochastic policies naturally.

# Prediction Vs Control

A prediction task in RL is where the policy is supplied, and the goal is to measure how well it performs. That is, to predict the expected total reward from any given state assuming the function $$\pi(a \mid s)$$ is fixed.

A control task in RL is where the policy is not fixed, and the goal is to find the optimal policy. That is, to find the policy $$\pi(a \mid s)$$ that maximises the expected total reward from any given state.


# Source
[A CSDN blog about RL fundament](https://blog.csdn.net/weixin_46185085/article/details/107598554)

[Youtube video tutorial about RL fundament](https://www.youtube.com/watch?v=_j6pvGEchWU&list=PLzvYlJMoZ02Dxtwe-MmH4nOB5jYlMGBjr&index=2)

