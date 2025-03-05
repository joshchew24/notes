# Query Evaluation Plans
- a query can be evaluated using several different strategies, called **plans**
	- all should produce the exact same result
- precursor to [[Operator Evaluation]]
## Cost Assessment
- to assess I/O cost of a plan, charge for reading pages (data or index), as well as writing any *intermediate* results to disk
	- **never charge for writing final result**
		- often result is returned to application or displayed to user, not written to disk
		- this value is the same for all plans, so does not affect ranking
- assess the [[Reduction Factor]]