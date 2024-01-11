# Code Smells
- properties of code that are symptomatic of specific deficiencies
	- often structural deficiencies
		- i.e. structural inhibitors to change
	- readability deficiency
		- consequently, code is harder to evolve and change
- considered a form of [[Tech Debt]]
- smells often arise when code evolve
	- defects are fixed and new futures are added
	- original design is obfuscated
	- new additions introduce coupling and reduce cohesion

## Types
### Duplicated Code
- code is duplicated throughout the system
- in same class
	- *extract method*
	- all copies within class now use single implementation
- in related classes
	- *push up*
	- classes now use common supertype
- in unrelated classes
	- often requires more complex analysis
### Long Method
- often indicates [[Single Responsibility Principle|SRP]] violation
### Large Class
- often indicates [[Single Responsibility Principle|SRP]] violation
### Long Parameter List
### Divergent Change
- often indicates [[Single Responsibility Principle|SRP]] violation
### Shotgun Surgery
- often indicates [[Single Responsibility Principle|SRP]] violation
### Feature Envy
### Middle Man