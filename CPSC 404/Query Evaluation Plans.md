# Query Evaluation Plans
- a query can be evaluated using several different strategies, called **plans**
	- all should produce the exact same result
## Cost Assessment
- to assess I/O cost of a plan, charge for reading pages (data or index), as well as writing any *intermediate* results to disk
	- **never charge for writing final result**
		- often result is returned to application or displayed to user, not written to disk
		- this value is the same for all plans, so does not affect ranking
## Generic Technique for Operator Evaluation - Physical Algebra for Relational Databases
- #todo change this fucking title lol
- 
- Indexing
	- use index to examine only tuples satisfying given selection/join condition
	- also called index probe
	- i.e. use index to go directly to 