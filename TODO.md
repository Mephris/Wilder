- Format saving the events
- Genes 
- Remember to, for now, save the already done events in the **Entity**. (Probably by ID)


#### 1. Foundation & Data Layer
- [ ] **Implement `SpeciesTemplate` & `SpeciesLibrary`**: Define base stats (Max Age, Action Speed, Diet) for rabbits, etc.
- [ ] **Enhance `Entity`**: 
    - Add `CurrentAge`, `CurrentHP`, and `Species` reference.
    - Expose `Genes` list and add logic to retrieve modifiers.
    - Add `CurrentGoal` and `CurrentBehavior` placeholders.
- [ ] **Implement `EntityLocation` functions**: Add `MoveTo` and clamping for coordinates.

#### 2. The "Pulse" (Tick System)
- [ ] **Implement `EntityTickHandler`**:
    - Logic for Hunger increase over time.
    - Logic for Fatigue increase.
    - Passive HP recovery/starvation damage.
    - Age progression.
- [ ] **Complete `EntityParameters`**: Implement logic for `GetCurrentHunger()` and add `Fatigue` and `Fear` properties.

#### 3. The "Brain" (Utility & Behavior)
- [ ] **Create `EntitiesGoalHandler` (The Utility Layer)**:
    - Implement the "Utility Decision" logic where weights are calculated based on needs (Hunger vs Fear).
    - Add `EntityGoal` class to store the result of the decision.
- [ ] **Implement `EntitiesBehaviourHandler` (The Tactics Layer)**:
    - Convert `CurrentGoal` into specific actions (e.g., if Goal = Eat, then Action = Move to Food).
- [ ] **Implement `PerceptionSystem`**: Proximity checks to find "Food" or "Predators" nearby.

#### 4. Infrastructure & Integration
- [ ] **Fix `EntityManager.AddEntity`**: It currently does not compile/work because it lacks parameters. It should support spawning from a `SpeciesTemplate`.
- [ ] **Implement `addGeneToEntity`**: Method in `EntityManager` to modify an existing entity's DNA at runtime.
- [ ] **Connect `OutputManager`**: Hook into `EntityManager` events to sync visual entities in the sandbox with the logic data.

#### 5. Gene Math
- [ ] **Implement Gene Modifiers**: Create a utility to apply $k$ values (Multiplier, Exponent, Offset) to raw parameters during Ticks and Utility decisions.

### ❓ To Be Decided (Research & Design)
- [ ] **Plants**: How are plants represented? (Entities, Tile data, or static objects?)
- [x] **BT vs Utility Boundary**: Define "Inertia" logic to prevent goal flickering and identify who owns the BT state.
- [ ] **Death Behavior**: When an animal reaches Max Age, does it leave a corpse? Can corpses be eaten by carnivores?
- [x] **Time Context**: Should the simulation use wall-clock time (`Sandbox.Time`) or a deterministic `TickCount` for logic?
- [ ] **Spatial System**: Design proximity query logic for a single biome (Optimization vs Simplicity).
