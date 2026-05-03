# Multi-Cloud Resource Allocation using Reinforcement Learning

## Overview
In modern multi-cloud environments, a Cloud Management Broker (CMB) must efficiently allocate incoming tasks across multiple cloud providers with varying capacities and costs. This project implements an intelligent resource allocator that balances two distinct types of services across three cloud instances. 

The system is designed to maximize successful task execution rewards while optimizing pre-provisioned resource utilization and minimizing the use of expensive on-demand fallback resources. The solution was developed in two phases: mathematical modeling via a Markov Decision Process (MDP) and dynamic AI training using Q-Learning.

## System Architecture & Constraints
The environment simulates real-world cloud routing with strict resource parameters and hardware constraints:

* **Cloud Providers (Static Capacities):**
  * Cloud 1: $0.5$ units
  * Cloud 2: $0.4$ units
  * Cloud 3: $0.4$ units
* **Task Specifications:**
  * **Task Type 1:** Requires $0.1$ units. Arrival rate ($\lambda_1$) = $3.51/min$, Service rate ($\mu_1$) = $0.90/min$.
  * **Task Type 2:** Requires $0.2$ units. Arrival rate ($\lambda_2$) = $1.17/min$, Service rate ($\mu_2$) = $0.30/min$.
* **Hardware Constraint:** Task Type 2 is inherently incompatible with Cloud Provider 2 and cannot be allocated there.

## Implementation Milestones

### Phase 1: Markov Decision Process (MDP) Modeling
To map the theoretical limits of the environment, a complete MDP was constructed to evaluate all valid state-action pairs:
* **State Space Generation:** Calculated all valid combinations of task distributions across the three clouds without exceeding the hardware capacity constraints.
* **Transition Rate Matrix (Generator Matrix):** Computed the exact rates of moving from one state to another based on the exponential arrival and service distributions.
* **Transition Probabilities:** Applied uniformization to convert the continuous-time transition rates into discrete-time transition probabilities.

### Phase 2: Q-Learning Agent
A Reinforcement Learning agent was deployed to learn the optimal allocation policy dynamically without relying on the a priori transition matrices:
* **Dynamic Reward System:** The agent receives weighted rewards based on maximizing the utilization ratios of the pre-provisioned cloud resources while penalizing sub-optimal routing. 
* **Simulation Environment:** Built a custom step-based environment that progresses time based on randomized exponential arrival and departure events.
* **Training Parameters:** Trained over 10,000 episodes using an $\epsilon$-greedy exploration strategy ($\epsilon$ decay = $0.99$), a learning rate ($\alpha$) of $0.1$, and a discount factor ($\gamma$) of $0.9$.
* **Policy Extraction:** Successfully converged to extract an optimal routing dictionary, mapping every possible environment state to the highest-reward routing action.

## 🛠️ Tech Stack
* **Language:** Python
* **Libraries:** NumPy, Matplotlib, Itertools, Random
* **Techniques:** Q-Learning, Markov Decision Processes, Queuing Theory, Uniformization
