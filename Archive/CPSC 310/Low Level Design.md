# Low Level Design

### Encapsulate What Varies
- identify which parts of the system are prone to future modification
- encapsulate these parts to ease future extension

### Design to Interfaces
- coupling to concrete implementations inhibits encapsulation
- designing systems around interfaces enables evolving and replacing concrete implementation

### Favour Composition Over Inheritance
- inheritance means that types must extend other concrete classes
- composition/delegation is better for long term evolvability
- ![[Pasted image 20231201003659.png]]


### subtopics
[[Design Patterns]]