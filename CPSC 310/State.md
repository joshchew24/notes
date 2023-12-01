# State
## Goal
- encapsulate states in objects
- let an object alter its behaviour when its internal state changes
- let an object appear as if it has changed its class
## Common Code Smells / Violations
- hard to predict all possible object states
- object state is scattered across fields and parameters
- divergent change and shotgun surgery - violates #srp 
	- as more states are added, we have to modify many methods
	- adding state is divergent change
- switch-on-type for states - violates #ocp 
- hard to add new states - violates #ocp 
- large class has many responsibilities - violates #srp 
- relying on concrete implementation and not abstraction - violates #dip 

## Implementation
![[Pasted image 20231201040012.png]]
- encapsulate states in objects
- on behaviours, state is updated
	- ![[Pasted image 20231201040035.png]]

### Pattern Details
- often used for problems that are natural to define using state machines
- create a hierarchy of possible states
- store a reference to one of the state objects representing the current state in a context
- delegate the state-related work to state objects
- to transition the context to another state, replace the current state object
	- context is *[[composition|composed]]* of states

## Abstraction
- abstract state class
## Polymorphism
- methods defined in the state class that require the affected behaviour
	- e.g. `pressHome()` and `pressPower()`
- methods defined in the abstract state class that differ depending on which state context is currently in
- invoked when system transitions from one state to another

## State vs [[Strategy]]
- state is almost an extension of Strategy
	- both use *composition* and *delegation*
- strategies are unaware/independent of each other
	- states normally know about each other
- strategies are different implementations to achieve a similar goal
	- states encapsulate entirely different behaviours
- strategies usually (not always) remain fixed during runtime 
	- states are usually replaced during runtime
- ![[Pasted image 20231201040611.png]]
- 