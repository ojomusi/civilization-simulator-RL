**Authors**  
Rukevwe Omusi

#### **Introduction**

The *Civilization* series is a turn-based strategy game that tasks players with managing empires over time. Success requires balancing warfare, resource management, diplomacy, and technological progress. For this project, I modified a simplified version of *Civilization* created by GitHub user *ben9583* to explore how reinforcement learning (RL) can enhance AI decision-making.

I aimed to solve the integration of deep Q-learning into this simulator to enable the AI to outperform the CPU’s automated processes. By doing so, I sought to model adaptive decision-making and explore how RL can handle complex, strategic choices in constrained environments.

---

#### **Methodology**

**Deep Q-learning Implementation**  
Deep Q-learning was selected for its ability to enable agents to balance exploration and exploitation, which is crucial for a strategy game where decisions must adapt dynamically to new conditions. The implementation involved:

* **Q-values:** Represented the expected cumulative reward of taking specific actions in a given state.  
* **Neural Network:** A feedforward neural network was used to approximate the Q-function, enabling the agent to predict Q-values for each possible action.  
* **Replay Memory:** Stored previous experiences as tuples (*state*, *action*, *reward*, *next\_state*), allowing the agent to learn from diverse historical interactions.  
* **Experience Sampling:** Random batches of size 10 were drawn from a replay memory of size 1000 during training to minimize correlations between consecutive experiences and prevent overfitting to recent events.  
* **Training Parameters:**  
  * **Gamma (Discount Factor):** Set to 0.99 to prioritize long-term rewards.  
  * **Learning Rate:** 0.001 to ensure smooth and stable weight updates.  
  * **Batch Size:** 10 to allow efficient gradient updates while maintaining training stability.

  ---

**Agent Designs and Reward Structures**  
 To evaluate how different goals influenced AI performance, four agents were designed with distinct reward structures:

1. **Money Empire:**

   * Rewarded based on resource accumulation each turn.  
   * Encouraged economic growth but often neglected territorial expansion, as shown in the results.  
2. **Territory Empire:**

   * Rewarded based on the number of tiles controlled per turn.  
   * Prioritized land acquisition but sometimes sacrificed stability and resource management.  
3. **Balanced Empire:**

   * Combined rewards from resource accumulation and territory expansion.  
   * Aimed to optimize overall performance across both metrics.  
4. **Softmax Empire:**

   * Utilized the same reward structure as Balanced Empire but replaced the epsilon-greedy exploration strategy with Softmax.  
   * Actions were selected probabilistically based on their Q-values, enabling a more nuanced exploration of less obvious strategies.

In-game mechanics such as tile availability, stability thresholds, and proximity to other empires constrained each agent's decisions.

---

**Game Setup and Execution**

* **Simplified Civilization Simulator:** The simulator by *ben9583* provided a manageable scope for implementing and testing reinforcement learning.  
* **Action Space:** Agents could choose from five actions per turn:  
  1. Expand territory.  
  2. Make peace deals.  
  3. Stabilize internal affairs.  
  4. Declare war.  
  5. Invade by sea.  
* **Turn Structure:** Each agent was evaluated over approximately 250 turns. Actions were taken sequentially in sessions, with other non-AI empires following random or predefined rules.  
  ---

**Databricks Integration for Analysis**  
To evaluate agent performance beyond the game window, I integrated Databricks for data storage and visualization:

* **Delta Lake:** Stored empire statistics (e.g., territory size, economy, stability) across all turns for efficient querying and analysis.  
* **Visualization Tools:**  
  * **Plotly and Seaborn:** Generated graphs showing trends in metrics like economy and territory size over time.  
  * **Matplotlib:** Used for comparative analyses of peak performance and inter-agent gaps.

This approach allowed for clear insights into agent behavior and outcomes, bypassing the limitations of the simulator's default interface.

---

**Handling Challenges**  
 Running multiple agents in a single session often led to undesirable interference, with agents sabotaging each other's performance. To address this, I ran each agent in isolated sessions and aggregated the data post-execution for comparison. This solution ensured fair evaluations and prevented skewed results due to external factors.

---

**Evaluation Metrics**  
 Agents were evaluated using the following metrics:

1. **Peak Territory Size:** Maximum number of tiles controlled during the session.  
2. **Peak Economy:** Highest resource accumulation achieved.  
3. **Territory and Economy Gaps:** Differences between the agent's performance and the best-performing non-agent empire in the session.

Metrics like stability and age were not used due to their dependence on random game events, but qualitative observations were noted for additional insights.

This methodology enabled a structured and rigorous evaluation of each agent's strategy, providing actionable insights into the effectiveness of various reward structures and exploration strategies.

---

#### **Results**

**Performance Metrics**

| Agent | Peak Territory | Peak Economy | Territory Gap | Economy Gap |
| ----- | ----- | ----- | ----- | ----- |
| Money Empire | 359 | 1424 | 306 | \-27 |
| Territory Empire | 442 | 2232 | 309 | 181 |
| Balanced Empire | 661 | 2615 | 600 | 915 |
| Softmax Empire | 483 | 3903 | 404 | 1854 |

**Key Insights:**

* Money Empire struggled due to its narrow focus on economic rewards.  
* Territory Empire outperformed Money Empire, emphasizing the importance of territorial expansion.  
* Balanced Empire achieved the best overall performance, with the widest gap in territory size.  
* Softmax Empire excelled in economic growth due to probabilistic exploration.

---

#### **Conclusions**

This project demonstrates that deep Q-learning can simulate strategic decision-making in a simplified *Civilization* simulator. Balanced and Softmax agents performed best, showcasing the value of combining metrics and advanced exploration strategies.

Beyond the results, I learned how to integrate reinforcement learning into existing systems and utilize Databricks for comprehensive analysis. These tools and techniques can be adapted for other AI applications requiring dynamic, strategic decision-making.