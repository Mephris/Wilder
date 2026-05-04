1. List of Features Captured from GDD
	- Evolutionary system
		- Entities
			- Behavior tree
			- Genes
		- Extraction and Implanting
	- Interactions between Entities
	- Entities
	- UI
		- Creating removing entities
2. Systems
	- Interactions between entities
	- Entity tracker (ticking, creating, destroying, get information about entites)
	- Splicing genes
	- User interface

```mermaid
flowchart TD

	Input[Player Input]
	Output[Visual Output]
	BT[Behaviour Tree]
	CE[Create Entity]
	DE[Destroy Entity]
	ME["Modify Entity (Modify genes)"]
	Tick[Tick each entity]
	EA[Entity Action]
	
    Input --> CE
    Input --> DE
    Input --> ME
    CE --> Tick
    DE --> Tick
    ME --> Tick
    Tick --> BT
    BT --> EA
    EA --> Output 
```




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

class EntitiesActionHandler{
	+processEntityAction(Entity entity) : result
}

class EntitiesBehaviourHandler{
	+processEntityBehaviour(Entity entity) : result
}

class Entity{
	+Gene[] genes
	+EntityData hungerEtc
	+EntityAction nextAction
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

class EntityAction {
	+ActionType type : Enum
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
EntityManager --> EntitiesActionHandler : delegates
EntityManager --> EntitiesTickHandler : delegates
EntityManager --> EntitiesBehaviourHandler : delegates
EntitiesTickHandler --> Entity : processes
EntitiesActionHandler --> Entity : processes
EntitiesBehaviourHandler --> Entity : processes
Entity --* Gene : stores
Entity --* EntityLocation : connects
Entity --* EntityAction : connects
Entity --* EntityParameter : connects
EntityParameter -->  Entity : uses for calculations


```



```mermaid
classDiagram

class InputManager{
	+Event(entitydata, genesData) createEntity
	+Event(entityID) removeEntity
	+Event(entityID, geneID) addGeneToEntity
}

class EntityManager{
	+Dict(id, entity) entities
	+Event(data) updateDisplayedData
	
	+addNewEntity(entitydata, genesData)
	+removeEntity(entityID)
	+addGeneToEntity(entityID, geneID)
}

class OutputManager{
	+updateRelevantData(Dict(dataID, value) data)
}

InputManager --> EntityManager
EntityManager --> OutputManager
```
