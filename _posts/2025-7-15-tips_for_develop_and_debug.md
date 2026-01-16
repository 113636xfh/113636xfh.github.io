---
title: "[Study Notes] Experience Summary for Coding and Debug (Ongoing) "
date: 2025-10-17
categories:
  - Study Notes
tags:
  - coding
  - debug
  - algorithms
toc_sticky: true
---

This is a personal summary of coding and debugging tips. I will update it when I find new ones.

### Use Official Libraries Instead of Reinventing the Wheel

When learning a new algorithm, we often reimplement it from scratch to deepen understanding and gain a sense of achievement. That is useful for learning, but in production or research you should prefer vetted libraries.

Personal implementations almost always hide bugs or perform poorly.

**The cost of debugging grows roughly with the square of code length.** Favor reliable libraries whenever possible.

### Code with Debugging in Mind

Most of my work is algorithmic, not app development. Algorithm code often embeds heavy math, which makes debugging harder than typical app code.

Design the implementation with one goal: make it easy to debug.


### Use Logging

For compute-intensive or long-iteration code, some bugs surface only after many steps. Prefer a logging framework: record intermediate values to files and reconcile them carefully, rather than relying on scattered `print` statements.


---

## Naming Rules

| Object Type   | Style                    | Semantics                     | Examples                                         |
| ------------- | ------------------------ | ---------------------------- | ------------------------------------------------ |
| Class         | `CamelCase`              | Noun / noun phrase            | `DataLoader`, `ReplayBuffer`, `PPOTrainer`       |
| Function/Method | `snake_case`           | Verb or verb phrase           | `load_data`, `compute_loss`, `save_checkpoint`   |
| Variable      | lower_snake_case         | Noun expressing data meaning  | `batch_size`, `episode_reward`                   |
| Constant      | UPPER_SNAKE_CASE         | Module-level readonly         | `MAX_STEPS`, `EPSILON`, `DEVICE`                 |
| Boolean       | `is/has/should/can_*`    | State or condition            | `is_valid`, `has_grad`, `should_stop`            |
| Module/Package| lower_snake_case         | Function/domain               | `models`, `datasets`, `optim`                    |
| File name     | lower_snake_case         | Matches main purpose          | `train.py`, `eval.py`                            |

                  |
| Temp variable | Meaningful abbreviation  | Avoid `tmp1/tmp2`             | `buf`, `acc`, `grad_norm`                        |
| Loop counter  | Conventional             | Short but clear               | `i`, `idx`, `step`, `epoch`                      |

---

### Loop Variable Conventions

| Scenario        | Recommended Names | Notes                     |
| --------------- | ----------------- | ------------------------- |
| Simple counting | `i, j, k`         | Local, short-lived        |
| Iteration index | `idx`             | Dataset / batch index     |
| Time step       | `step`            | RL / simulation           |
| Training epoch  | `epoch`           | Training loop             |
| Sample          | `sample`          | Single data item          |
| Batch           | `batch`           | Batch of data             |

---

### Common RL Naming

| Meaning     | Recommended Name |
| ----------- | ---------------- |
| Observation | `obs`            |
| Action      | `act`            |
| Reward      | `rew`            |
| Next state  | `next_obs`       |
| Done flag   | `done`           |
| Log prob    | `log_prob`       |
| State value | `value`          |
| Advantage   | `advantage`      |
| Return      | `return_`        |
