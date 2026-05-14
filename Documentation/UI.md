### What is needed
- View list of animals
	- Grouping by Bioms, Genes
	- Filtering
	- Edit the entity
	- Remove the entity
- View of messages what happened
	- Grouping
	- Filtering
	- Using a event storage system
- Simulation Speed control
- Preinstaled and saved by player entity templates
- Gene list
	- add to currently edited entity 
- Currently edited entity type
	- Name
	- Release into wild and where
	- Remove installed genes
	- Cancel edit
- Currently selected event more info
### What is not needed for prototype
- Grouping, filtering, just show a list
- Events just as a string
- gene and preset list can be a expandable list
### First draft

![[UI draft 1]]


```mermaid
classDiagram

class SelectedEntityView{
	+List<int> geneIds
	+Objects parameters
}

class GeneViewUI{
	+List<GeneUI> shownGeneUIs
	+Event<int> OnGeneUIAdd 
}

class GeneUI{
	
}

class EntityPresetViewUI{
	+List<EntityPresetUI> shownEntitiesUIs
	+Event<int> OnEntitiesUIAdd
}

class EntityPresetUI{
	
}

class TimeControlViewUI{
	
}

class EntitiesViewUI{
	
}

class EventViewUI{
	+ShowNewEvent(string event)
}

SelectedEntityView --> GeneViewUI : References
GeneViewUI --* GeneUI : Holds
SelectedEntityView --> EntityPresetViewUI : References
EntityPresetViewUI --* EntityPresetUI : Holds
SelectedEntityView --> EventViewUI : References
SelectedEntityView --> TimeControlViewUI : References
SelectedEntityView --> EntitiesViewUI : References

```

### Time Control

**Visual**
![[UI time control draft]]

**Code**
- On each button pressed --> notify TimeManager

### Event View

**Visual**


**Code**
