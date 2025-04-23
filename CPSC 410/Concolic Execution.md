# Concolic Execution
- combination of [[Symbolic Execution]] and [[Dynamic Program Slicing|Dynamic Program Analysis]]
- concolic = **conc**rete + symb**olic**
- **during** concrete execution, also **calculate and record symbolic states** and **path conditions**
	- analysis state contains
		- symbolic state
		- path conditions
		- concretization conditions
## Steps
1. 