---
title: "[Study Notes] Framework for Optimization Problems and Algorithms"
date: 2023-11-27
categories:
  - Study Notes
tags:
  - optimization
  - math
  - numerical-methods
  - algorithms
toc_sticky: true
---

Study notes on optimization problems and algorithms.

## Optimization Problem

When we face a optimization problem, firstly, we should figure out what kind of problem it is, then, we can conveniently search solve method for this problem.Optimization problems can be classified into various categories based on different criteria. Here are some common ways to classify optimization problems:

### By the Nature of the Variables:

1. **Continuous Optimization**:
   - Problems where the variables can take any value within a given range. 
   - Typically involves calculus-based methods.
2. **Discrete Optimization**:
   - Problems where the variables are restricted to be integers or take on a finite set of values.
   - Often involves combinatorial methods.
3. **Mixed Integer Optimization**:
   - Problems that have both continuous and integer variables.
   - More complex and computationally challenging than continuous or purely discrete optimization.

### By the Nature of the Objective Function:

1. **Linear Optimization**:
   - The objective function and the constraints are linear.
   - Problems can be solved efficiently with linear programming techniques.
2. **Nonlinear Optimization**:
   - The objective function or constraints are nonlinear.
   - More complex and may require iterative methods.
3. **Quadratic Optimization**:
   - Problems where the objective function is quadratic and the constraints are linear.
   - Extensions of linear programming but with additional complexity.

### By the Number of Objectives:

1. **Single-Objective Optimization**:
   - The goal is to optimize a single objective function.
   - Most classical optimization problems fall into this category.
2. **Multi-Objective Optimization**:
   - Involves optimizing more than one objective function simultaneously.
   - Often requires trade-offs and the concept of Pareto optimality.

### By Constraints:

1. **Unconstrained Optimization**:
   - Problems without any constraints.
   - Relatively easier to solve compared to constrained problems.
2. **Constrained Optimization**:
   - Problems with one or more constraints.
   - Constraints can be linear or nonlinear, equality or inequality.

### By the Structure of the Problem:

1. **Convex Optimization**:
   - The feasible region is a convex set, and the objective function is a convex function.
   - Solutions are well-behaved and can be found efficiently.
2. **Non-Convex Optimization**:
   - The feasible region or the objective function is non-convex.
   - Solutions can be difficult to find due to local optima.

### By Complexity:

1. **Deterministic Optimization**:
   - Problems where all parameters are known with certainty.
   - Can be solved using deterministic algorithms.
2. **Stochastic Optimization**:
   - Problems where some parameters are uncertain or random.
   - Involves probabilistic models and often requires simulation.

## Optimization Method

### Global Optimization versus Local Optimization

- Local optimization involves finding the optimal solution for a specific region of the search space, or the global optima for problems with no local optima.
- Global optimization involves finding the optimal solution on problems that contain local optima.

Local optimization likely to get the extrema in the basin but may not the global optima. Global optimization trend to outside every basin but not good at find the extrema.


#### Local optimization
- Nelder-Mead Algorithm
- BFGS Algorithm
- Hill-Climbing Algorithm

- **Nelder-Mead** doesn’t use derivatives, making it suitable for non-smooth functions, while **BFGS** requires gradient information and works well for smooth, differentiable functions.
- **Hill-Climbing** is a simpler, local optimization algorithm, while **Nelder-Mead** and **BFGS** are more sophisticated and can handle more complex functions, especially with many variables.

#### Global optimization
- Genetic Algorithm
- Simulated Annealing
- Particle Swarm Optimization

### Methods for Continuous vs Discrete Variables

- Continuous and discrete decision spaces require different representations and operators. Many local methods assume differentiability and work best in continuous spaces; many global/metaheuristic methods can operate in both but often need problem-specific encodings and neighborhood operators.

#### Continuous domains
- Local (often gradient-based): Gradient Descent/Adam, Newton/Quasi-Newton (BFGS, L-BFGS), Conjugate Gradient, Trust-Region, Sequential Quadratic Programming (SQP); derivative-free local: Nelder–Mead, Pattern Search, CMA-ES (local/global depending on settings).
- Global (black-box friendly): CMA-ES, Differential Evolution (DE), Particle Swarm Optimization (PSO), Simulated Annealing (SA), Bayesian Optimization (Gaussian Processes/Tree-structured Parzen for low-dim expensive problems).
- Constraints: penalty/barrier methods, augmented Lagrangian, projection (projected gradient), filter/SQP, repair heuristics for metaheuristics.

#### Discrete / combinatorial domains
- Exact/complete: Integer Linear Programming (ILP) with Branch-and-Bound/Cut, Constraint Programming (CP), Dynamic Programming (for special structures).
- Local/metaheuristics: Local Search with problem-specific neighborhoods (e.g., 2-opt/3-opt for routing), Tabu Search, Simulated Annealing (commonly effective for discrete), Genetic Algorithms (permutation/bitstring encodings), Ant Colony Optimization (ACO), GRASP.
- Representation matters: choose encodings and move operators that preserve feasibility (e.g., swap/insert for permutations; set add/remove for subsets).

#### Mixed-integer (MINLP/MILP)
- MILP/MINLP solvers (Branch-and-Bound/Cut, Outer Approximation) for convex/linearizable parts.
- Hybrid approaches: use metaheuristics to choose discrete decisions, and run a continuous solver (SQP/Interior-Point) to optimize continuous variables (a.k.a. matheuristics).

#### Choosing methods in practice
- Differentiable and smooth, moderate dimension (≤10–100): prefer gradient-based (BFGS/L-BFGS, TR/SQP). If gradients unavailable or noisy, try derivative-free local (Nelder–Mead, pattern search).
- Expensive black-box with very low dimension (≤20) and few evaluations: Bayesian Optimization; otherwise CMA-ES/DE.
- High-dimensional continuous (≥100): L-BFGS (when gradients available), or CMA-ES with diagonal/active covariance, or scalable DE/PSO variants.
- Strong combinatorics/constraints: model as ILP/CP first; if intractable, use SA/Tabu/GA/ACO with feasibility-preserving moves and penalty/repair.
- Constraints: prefer projection or augmented Lagrangian for continuous; for discrete, use repair operators or decode-then-project strategies.

#### Metaheuristics (overview)

Why “metaheuristics”: “meta” (Greek, “beyond/at a higher level”) + “heuristic” (a problem-specific rule of thumb). Metaheuristics are higher-level search strategies that guide or orchestrate underlying heuristics/local searches to balance exploration and exploitation. They serve as problem-independent templates that adapt via representations, neighborhoods, and evaluation functions; in return for scalability and robustness, they typically do not guarantee global optimality.

<!-- - 中文小注：之所以称为“元启发式（Metaheuristics）”，是因为它位于具体启发式之上，充当通用的高层搜索策略，用来协调探索与开发，通常不保证全局最优，但具有良好的通用性与可扩展性。 -->

- Population-based: GA (crossover/mutation), DE (differential mutation), PSO (velocity updates), CMA-ES (adaptive covariance sampling).
- Single-trajectory: SA (temperature schedule), Tabu Search (memory), Iterated Local Search.
- Construction-based: ACO, GRASP.

Tips: scale/normalize variables, seed from heuristics, use restarts, log best-so-far, and validate with multiple random seeds. Combine global exploration (e.g., DE/SA) with local refinement (e.g., BFGS/SQP) for robust performance.


<!-- ## Complex Optimization Method Debug Tips

Write soon -->
