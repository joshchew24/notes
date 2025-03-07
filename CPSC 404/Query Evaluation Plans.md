# Query Evaluation Plans
- a query can be evaluated using several different strategies, called **plans**
	- all should produce the exact same result
- uses [[Operator Evaluation]]
- see also [[Operator Tree]]
## Cost Assessment
- to assess I/O cost of a plan, charge for reading pages (data or index), as well as writing any *intermediate* results to disk
	- **never charge for writing final result**
		- often result is returned to application or displayed to user, not written to disk
		- this value is the same for all plans, so does not affect ranking
- assess the [[Reduction Factor]]
## Plan
- [[Operator Tree]] of operations from [[CPSC 404/Relational Algebra|Relational Algebra]]
	- each operation assigned a specific algorithm/strategy
	- each operator typically implemented using a *pull* interface
		- when an operator is *pulled* for the next output tuples, it *pulls* on its inputs and computes/retrieves them
- **how do we find the best plan?**
	- what plans are considered?
	- how is the plan cost estimated?
	- 