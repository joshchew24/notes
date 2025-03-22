# Query Blocks
- unit of optimization
- SQL query is parsed into a collection of **query blocks**
	- optimized one block at a time
	- i.e. nested queries
		- nested blocks usually treated as **calls to a subroutine**, made **once per outer tuple**
## Evaluating Single Block Queries
- find "best" single relation access plan
- find "best" 2-relation (join) plan
- find "best" way of joining a $k$-relation plan to a $(k+1)^{th}$ plrelation
	- there may be more than one "best" plan
	- maintain all
	- use dynamic programming to find "best" plan for $k+1$ joins from previous plans