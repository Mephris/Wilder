### General Loop
From [[TDD_Animals]]

```mermaid
flowchart TD

	Input[Player Input]
	CE[Create Entity]
	DE[Destroy Entity]
	ME["Modify Entity (Modify genes)"]
	LL[LifeLoop]
	Tick[Tick each entity]
	
    Input --> CE
    Input --> DE
    Input --> ME
    CE --> Tick
    DE --> Tick
    ME --> Tick
    Tick --> LL
     
```

##### Life Loop
```mermaid
flowchart TD
	subgraph "The Life Loop"
	    Utility[<b>Utility Decision</b><br/>Calculate Weights]
        Switch{<b>Switch Goal</b><br/> The Switch containing current goal}
        BT[<b>Behavior Tree</b><br/>Execute Subtree]
        Output[<b>Visual Output</b><br/>Anims / Movement]
    
        Utility --> Switch
        Switch -- "New Goal" --> BT
        Switch -- "Same Goal" --> BT
        BT --> Output
        Output -- "Tick Callback" --> Utility
        end
```

### System Architecture


```mermaid
classDiagram

class EntityManager{
	+Dict(id, entity) entities
	+Event(data) updateDisplayedData
	
	+addNewEntity(entitydata, genesData)
	+removeEntity(entityID)
	+addGeneToEntity(entityID, geneID)
}

class EntitiesTickHandler{
	+tickEntities(Entity[] entities) : result
}

class EntitiesGoalHandler{
	+processEntityGoal(Entity entity) : result
}

class EntitiesBehaviourHandler{
	+processEntityBehaviour(Entity entity) : result
}

class Entity{
	+Gene[] genes
	+EntityData hungerEtc
	+EntityGoal currentGoal
	+EntityBehaviour entityBehaviour?
}

class Gene{
	+GeneType type : Enum
	+Object[] traits
	
	+getFloat(id) : float
	+getBool(id) : bool
	+getInt(id) : int
}

class EntityLocation {
	+sbyte elevation
	+sbyte humidity
	+sbyte roughness
	+sbyte temperature
	
	+compare(EntityLocation) : bool
}

class EntityGoal {
	+GoalType type : Enum
	+Object[] traits
	
	+getFloat(id) : float
	+getBool(id) : bool
	+getInt(id) : int
}

class EntityParameter {
	+Entity parent
	+float baseParameter
	
	+getCurrentParameter():float
}

EntityManager --* Entity : stores
EntityManager --> EntitiesGoalHandler : delegates
EntityManager --> EntitiesTickHandler : delegates
EntityManager --> EntitiesBehaviourHandler : delegates
EntitiesTickHandler --> Entity : processes
EntitiesGoalHandler --> Entity : processes
EntitiesBehaviourHandler --> Entity : processes
Entity --* Gene : stores
Entity --* EntityLocation : connects
Entity --* EntityGoal : connects
Entity --* EntityParameter : connects
EntityParameter -->  Entity : uses for calculations
```

### (ForLater)Perception & Sensory systems
To keep it simpler then messing with LOS or other BS, we will be using proximity-based detection. 

##### Query logic
Every **PerceptionInterval**, the animal asks the **SpacialSystem** 
	*"What are the nearest entities within **SmellRadius**"*
- **Logic:** Simple distance check between **Entity.Position** and **Target.Position**

##### Data Storage
Results are stored within the list inside the **Entity** as **PerceptionData**

```mermaid
classDiagram

class PerceptionData{
	+visibleFood: Entity[];
	+visiblePredators: Entity[];
	+nearestWater: Vector3 | null;
}
```
##### Utility Interaction
[[AI#Utility Layer (The "Brain")|Utility]] uses this data to calculate weights 
	*Examples:*
	- If *visiblePredator* is not empty, set *FearWeight* to 1.0
	- If *visibleFood* has 3 items, set *FoodAvailabilityScore* to 1.0