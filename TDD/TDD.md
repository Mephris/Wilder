### Code policy
>**UI** 
>**Architecture**
>Entity should only hold references to other classes, and all things con
### Think though list
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

![[TDD_Animals#System Architecture|clean]]



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

