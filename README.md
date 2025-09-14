# Bandit Batsman: Optimizing Cricket Strategy with Multi-Armed Bandits 

![Python](https://img.shields.io/badge/Python-3.7%2B-blue.svg)


This project explores the application of **Multi-Armed Bandit (MAB) algorithms**—a set of powerful techniques for decision-making under uncertainty—to discover optimal batting strategies in a simulated cricket environment. The goal is to develop an intelligent agent that can make decisions to maximize runs, minimize wickets, or optimize for custom performance metrics.



---

## Core Concept: The Batsman's Dilemma

A batsman at the crease constantly faces an **exploration-exploitation dilemma**:
* **Exploit:** Play a shot that has historically yielded good results.
* **Explore:** Try a different, potentially riskier shot that might lead to a better long-term outcome.

This project models this dilemma by framing different batting actions as "arms" of a multi-armed bandit. The agent (the batsman) must learn the underlying reward and risk probabilities of each action by playing a series of balls.

---

## Problems Explored

The project is broken down into five distinct problems, each with a unique objective for the MAB agent to solve. The available actions are $A = \{0, 1, 2, 3, 4, 6\}$, representing the intended run.

### 1. Wicket Minimization (Defensive Strategy)
* **Objective:** To find the batting action `a` that minimizes the probability of getting out, $p_{out}(a)$.
* **Metric:** Minimize the cumulative regret $R_n = \sum_{t=1}^{n}(w(a_t) - w^*)$, where $w(a) = p_{out}(a)$ is the expected wickets per ball and $w^*$ is the minimum possible wicket probability.

### 2. Run Maximization (Aggressive Strategy)
* **Objective:** To find the action `a` that maximizes the expected runs per ball.
* **Metric:** Maximize the cumulative reward by minimizing regret $R_n = \sum_{t=1}^{n}(s^* - s(a_t))$, where the expected score is $s(a) = (1 - p_{out}(a)) \cdot p_{run}(a) \cdot a$.

### 3. Runs-per-Wicket Efficiency
* **Objective:** To find the action `a` that maximizes the trade-off between scoring runs and the risk of getting out.
* **Metric:** Maximize the "runs gained per wicket" metric by minimizing regret $R_n = \sum_{t=1}^{n}(\tau^* - \tau(a_t))$, where $\tau(a) = \frac{(1 - p_{out}(a)) \cdot p_{run}(a) \cdot a}{p_{out}(a)}$.

### 4. Realistic Match Simulation
* **Objective:** Move from theoretical regret to practical performance. The agent must maximize the total score across multiple matches.
* **Constraints:** Each match has a fixed number of balls (60) and wickets (4). The match ends if either resource is exhausted.
* **Goal:** Maximize the total score aggregated over all simulated matches, $\sum_{m=1}^{M} s_m$.

### 5. Contextual Bandits for Team Strategy 
* **Objective:** This problem introduces **context** in the form of different batter types. The outcome probabilities, $p_{out}(a, i)$ and $p_{run}(a, i)$, now depend on both the action `a` and the current batter `i`.
* **Dual-Decision Problem:** The agent must make two decisions:
    1.  Select an action $a_t$ for the current batter.
    2.  When a wicket falls, select the next best batter to send in.
* **Goal:** Maximize the total team score across `M` matches, showcasing a more complex, state-aware strategic ability.

---

## Algorithms Implemented
This repository implements and compares several MAB algorithms to solve the problems above:
* **Epsilon-Greedy:** A simple baseline for balancing exploration and exploitation.
* **Upper Confidence Bound (UCB1):** An optimistic algorithm that explores less-played actions based on their potential.
* **Thompson Sampling:** A Bayesian approach that uses probability distributions to guide action selection.
* **KL-UCB:** An advanced algorithm that uses Kullback-Leibler divergence for more efficient exploration, particularly effective for minimizing regret.

---



## Results and Visualizations 
The performance of the algorithms is evaluated using two primary visualizations:

1.  **Cumulative Regret Plots (Problems 1-3):** These plots show how the "loss" compared to an optimal strategy accumulates over time. A better algorithm will have a flatter curve, indicating faster learning.
    

2.  **Total Score Histograms (Problems 4-5):** These plots show the distribution of final scores across thousands of simulated matches. A better strategy will result in a distribution shifted towards higher scores.
