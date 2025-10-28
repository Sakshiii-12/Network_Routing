# Intelligent Network Routing using Reinforcement Learning

## Overview
This project implements an **intelligent adaptive routing system** using **Q-learning**, a Reinforcement Learning (RL) algorithm, to enhance computer network performance. Unlike traditional algorithms such as **Shortest-Path (Dijkstra)** that follow static paths, this system allows routers (agents) to **learn dynamically** which routes are optimal based on latency, congestion, and delivery success.  

Each node acts as a decision-making agent that improves its strategy through **trial and reward**. Over time, the network learns to avoid congested links, balance load, and deliver packets more efficiently.



## Key Concepts

### Reinforcement Learning for Network Routing
Routing is modeled as a **Markov Decision Process (MDP)** defined by:

$$
\mathcal{M} = (S, A, P, R, \gamma)
$$

Where:  
- $$\( S \)$$: state space — $$\((\text{current node}, \text{destination})\)$$  
- $$\( A \)$$: action space — possible next-hop neighbors  
- $$\( R \)$$: reward — positive for successful delivery, negative for loops or drops, small penalty for latency  
- $$\( \gamma \)$$: discount factor for future rewards  


### Q-Learning
Q-learning estimates the optimal state–action value function $$\( Q^{*}(s,a) \)$$ without prior network knowledge.  
For each transition $$\((s, a, r, s')\)$$, the Q-values are updated as:

$$
Q(s,a) \leftarrow Q(s,a) + \alpha [r + \gamma \max_{a'} Q(s',a') - Q(s,a)]
$$

Over time, the agent converges to $$\( Q^{*}(s,a) \)$$ and selects actions using an **ε-greedy** policy — exploring with probability $$\( \epsilon \)$$ and exploiting the best-known action otherwise.



## Dependencies

Install all required Python libraries before running the simulation:

```bash
pip install networkx numpy matplotlib pandas tabulate
````



## How It Works

### Network Setup

A **14-node NSFNET-like topology** is created using *NetworkX*, with latency-weighted edges representing transmission delay.

### Training Phase

Each episode simulates packet transfer from a random source to destination:

1. Agent selects the next-hop neighbor using ε-greedy strategy.
2. Receives reward for delivery, penalty for loop or drop.
3. Updates its Q-value.
4. Repeats until packet delivery or hop limit is reached.

### Evaluation Phase

After training, three routing strategies are compared:

* **Q-learning (Greedy)** — learned adaptive policy
* **Shortest-Path (SP)** — Dijkstra’s fixed routing
* **Random (RR)** — random next-hop selection

**Performance Metrics:**

* **Packet Delivery Ratio (PDR):**

$$
\text{PDR} = \frac{\text{Packets Delivered}}{\text{Packets Generated}}
$$

* **Average Hops:**

$$
\text{Avg Hops} = \frac{\sum \text{Hops}}{\text{Packets Delivered}}
$$

* **Mean Link Utilization:** average link usage per tick.

A **scaling study** evaluates these metrics under varying packet generation rates to test robustness under congestion.



## Output and Visualization

After running the program, several graphs are generated sequentially.

| Graph                  | Description                       |
| ---------------------- |---------------------------------- |
| Topology Plot          | NSFNET-like graph with latencies. |
| Training Curve         | Reward trend over episodes.       |
| PDR Comparison         | Q-learning vs SP vs Random.       |
| Avg Hops Chart         | Average hops comparison.          |
| Link Utilization Chart | Mean link usage.                  |
| PDR vs Rate            | PDR under increasing traffic.     |
| Hops vs Rate           | Hop count vs packet rate.         |
| Q-Agent Heatmap        | Edge usage frequency.             |
| Link Usage Overlay     | Traffic intensity per link.       |



## Theoretical Insight

In conventional routing, the goal is to minimize total path cost:

$$
C(p) = \sum_{(u,v) \in p} \text{latency}(u,v)
$$

Q-learning instead **maximizes cumulative rewards** over time:

$$
R_t = \sum_{k=0}^{T} \gamma^k , r_{t+k}
$$

This transforms routing into a **sequential decision process**, where nodes autonomously adapt their behavior.
The Q-values inherently encode:

* Low-latency path preference
* Congestion avoidance
* Loop prevention

Thus, intelligent routing emerges from local learning without centralized control.



## Example Results (Typical)

| Agent             | PDR   | Avg Hops | Mean Link Utilization |
| ----------------- | ----- | -------- | --------------------- |
| **Q-learning**    | ~0.92 | ~4.3     | ~0.028                |
| **Shortest-Path** | ~0.88 | ~3.9     | ~0.030                |
| **Random**        | ~0.47 | ~6.8     | ~0.046                |

> *Note: Results may vary slightly based on random seed and training episodes.*



## Conclusion

The **Q-learning-based routing system** learns efficient, congestion-aware paths through continuous interaction with the network.
Compared to shortest-path and random routing, it provides:

* Higher delivery ratios
* Balanced hop counts
* Improved link utilization

