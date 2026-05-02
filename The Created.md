A creature, plant or beast made by a player or mutated by them. Counted towards the players environmental changes. 

### AI
AI for the animals will be a mix of two things, Utility and Behavior Trees. This hybrid approach allows for complex, trait-driven decision making while maintaining the performance required for ~1000 entities.

#### Utility Layer (The "Brain")
The Utility layer is responsible for high-level goal selection. Instead of hardcoded transitions, entities evaluate their "Needs" and "Traits" to determine the most urgent action.

##### Needs
The instinct which choses the actions of creatures within the game, updated 

| Need    | Description                                                                    | Used in Actions |
| ------- | ------------------------------------------------------------------------------ | --------------- |
| Hunger  | How close is the creature to starvation?                                       |                 |
| Thirst  | How close is the creature to dying of thirst?                                  |                 |
| Fatigue | Stamina level of the creature, refreshes on sleep, used during other actions   |                 |
| Social  | How much does the creature need to socialize.                                  |                 |
| Fear    | Does creature feel threatened? usually connected to seeing stronger creatures. |                 |
##### Actions
Actions are taken depending on the current needs and their current weights, in case of making animals it will be actions for survival. 

Each action has its weight, calculated depending on the creatures current needs. 

Weight of action is calculated by: 

###### Normalizing values
Normalize the used values into decimal between 0 and 1. 
	Example: Hunger: 0.8

###### Choosing the response curve
The "How creature reacts", changing of how urgent each action will feel to the creature. 
- Linear (Exponent = 1) : 0.5 Hunger = 0.5 Score 
- Exponential (Exponent < 1) : 0.5 Hunger = 0.2 Score (Doesn't give a fuck until its real real hungy)
- Logarithmic (Exponent > 1) : 0.5 Hunger = 0.8 Score (It gets hungry real fast and is desperate)

###### Score
In order to calculate the final weight for an action, we multiply the factors, if any of the values is 0 the weight of an action becomes 0 as well. 
> Example (Hunt): Final-score = (Hunger-score * Prey-Availability-Score) * (1.0 - Fear-Score)
> **Hunger (0.8):** "I'm starving!"
> **Prey-availability (1.0):** "There's a rabbit right there!"
> **Fear (0.2):** "I'm a little scared." -> (1.0 - 0.2) = 0.8 (Confidence)
> 0.8 * 1.0 * 0.8 = 0.64 -> Final Score: 0.64



##### Trait Modifiers:
Most traits will be simply fine tuning of math in the Utility rather their own BT's. 

There are different types of traits we can do depending on what we want to fine tune in the behavior. 

###### The Exponent (Changing personality, sensitivity)
The traits that will impact how animals will be interacting with the world, the **Linear Curve** animals may feel predictable and robotic. 
*Linear will be the baseline.*

| Trait name | Need | Exponent | Description                             |
| ---------- | ---- | -------- | --------------------------------------- |
| Default    | Fear | 2.0      | Starts getting scared at medium danger. |

While **Exponential curve**, they will feel more *Desperate* with higher tolerance. For most time they will not care for the need until it reaches a high value, in which moment it will be the only thing they will be thinking about. 

| Trait name | Need | Exponent | Description                                                                       |
| ---------- | ---- | -------- | --------------------------------------------------------------------------------- |
| Brave      | Fear | 5.0      | Stays at "0 weight" until danger is literally touching it, then it finally spikes |
The **Logarithmic Curve** will make animals very sensitive and easily satisfied. 

| Trait name | Need | Exponent | Description                                            |
| ---------- | ---- | -------- | ------------------------------------------------------ |
| Coward     | Fear | 0.5      | Immediately hits "0.7 weight" at the slightest danger. |
###### Multiplier
The simple multiplication applied to the final score of a need. It's best for traits that represent **Metabolism** or **Obsession**

| Trait name | Need | Multiplier | Description |
| :--- | :--- | :--- | :--- |
| **Glutton** | Hunger | 1.5x | Food is 50% more important than anything else. |
| **Ascetic** | Hunger | 0.7x | Can ignore hunger for much longer. |
###### Offset
Changes the baseline for the character. 

| Trait name   | Need | Offset | Description                              |
| :----------- | :--- | :----- | :--------------------------------------- |
| **Paranoid** | Fear | +0.2   | Always feels a baseline level of danger. |
| **Zen**      | Fear | -0.2   | A baseline level of calm.                |
##### Goal Selection
The entity periodically calculates a score (0.0 to 1.0) for each potential goal. The highest-scoring goal is passed to the Behavior Tree.


#### 2. Behavior Tree Layer (The "Body")
The Behavior Tree handles the execution of the goal selected by the Utility layer. This keeps the BTs modular and focused on specific task logic.

- **Sub-Tree Switching:** When the Utility layer picks a goal (e.g., `Goal: Hunt`), the BT switches to the corresponding sub-tree.
- **Action Nodes:** Specific leaf nodes handle the "how-to": `PathToTarget`, `PlayAnimation`, `PerformAttack`.
- **Interruption:** If the Utility layer detects a high-priority change (e.g., "Fear" spikes due to a predator), it can force the BT to abort the current sub-tree and switch to `Goal: Flee`.

#### 3. Scalability & Optimization
- **AI Level of Detail (LOD):**
    - **LOD 0 (Near Player):** Full Hybrid AI (Utility + BT + Full Pathfinding).
    - **LOD 1 (Medium Distance):** Utility AI updates less frequently; simplified pathfinding.
    - **LOD 2 (Far/Off-screen):** Statistical simulation. Needs fluctuate based on math functions; entities "warp" between resource nodes rather than walking.
- **Staggered Ticking:** Utility evaluations are distributed across frames. Only a fraction of the population (e.g., 5-10%) performs a "Think" update per tick.
- **Spatial Partitioning:** Entities only "perceive" other entities within their local grid cell, drastically reducing the cost of sensory checks (e.g., looking for prey).
