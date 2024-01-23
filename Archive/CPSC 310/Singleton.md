# Singleton
## Goal
Restrict the instantiation of a class to one object. i.e. ***exactly one*** object is needed to coordinate actions across the system

## Client Implementation
![[Pasted image 20231201033253.png]]
## Singleton Implementation
- saves an instance of the class to a static variable
- statically provides access to that instance
- only creates instance if needed (=lazy instantiation)
- hides the constructor, so only the singleton class can call it
- ![[Pasted image 20231201033332.png]]
- ![[Pasted image 20231201033345.png]]

## Problems
- requires special treatment in multi-threaded environments
	- multiple threads need access to single instance
- introduce new global with dependencies spread throughout the codebase
	- introduces possible need for shotgun surgery
- violates #srp 
	- two responsibilities for singleton class
		- ensure only one instance is instantiated
		- provide a single way to get the instance
- violates #dip 
	- both client and singleton are tied to singleton class
- violates #ocp 
	- private constructor
		- not open for extension
		- difficult to mock in unit tests