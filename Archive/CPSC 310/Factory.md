# Factory
- extract construction logic into factory class
- client requests objects from the factory
	- different factory classes for each subtype
## Common Code Smells / Violations
- constructor does work outside of the scope of the class
- different responsibilities: building the object, and being the object - #srp violation
- constructor code is prone to changes

![[Pasted image 20231201031117.png]]
## Abstraction
- factory at the top of the factory hierarchy
- and *maybe* the product at the top of the product hierarchy
## Polymorphism
- create the object
	- client calls factory function instead of the object's constructor