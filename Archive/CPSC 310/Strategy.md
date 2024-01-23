# Strategy
## Goal
- vary *behaviour* in our system (i.e. algorithm) without knowing all possible algorithms in advance
- allow new algorithms to be added (open for extension) without impacting clients
- expose explicit interfaces for which we can provide new algorithms as new requirements are added
## Common Code Smells / Violations
- switch on type - violates #ocp 
- multiple algorithms implemented in one class - violates #srp 
	- divergent changes - if new algo were to be added, class complexity increases

## Implementation
![[Pasted image 20231201034743.png]]
- Potential #dip violation
	- compressionService/context depends on low-level concrete implementations of strategy
	- ![[Pasted image 20231201035001.png]]
## Advanced Strategy (with Factory)
![[Pasted image 20231201035145.png]]
- enhanced #dip
	- context does not depend on concrete strategies
- enhanced #ocp 
	- new strategies can be added and context is oblivious
- responsibility of knowing about concrete strategies moved to factory

## Abstraction
- strategy interface
	- implemented by concrete strategy classes
- possibly move strategy creation to Factory
## Polymorphism
- the method implementing the algorithm
	- e.g. `compress()`
- possibly: strategy creation if using Factory
## When do we use polymorphic methods?
- when algorithm is invoked
- possibly: when strategy is created if using Factory