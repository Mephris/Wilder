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


![[TDD_Animals#General Loop|clean]]


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

EntityManager --* Entity : stores
EntityManager --> EntitiesActionHandler : delegates
EntityManager --> EntitiesTickHandler : delegates
EntityManager --> EntitiesBehaviourHandler : delegates
EntitiesTickHandler --> Entity : processes
EntitiesActionHandler --> Entity : processes
EntitiesBehaviourHandler --> Entity : processes



Entity --* Gene
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

