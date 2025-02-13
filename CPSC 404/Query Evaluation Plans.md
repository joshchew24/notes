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
- these physical algebra operators are used to evaluate SQL queries
- all SQL queries (i.e. extended [[Relational Algebra]]) can be efficiently processed using just these 3 physical opreators
- **[[Indexing]]**
	- use index to examine only tuples satisfying given selection/join condition
	- also called index probe
- **[[Iteration]]**
	- **[[Table Scan]]**: scan a table (i.e. data pages) to retrieve tuples satisfying condition
	- **[[Index Scan]]**: scan index pages and examine *data entries* therein
		- **different from index probe**
		- typically need to read much less than table scan
- **[[Partitioning]]**
	- sorting (e.g. [[Merge Sort|External Sort]])
	- hashing (e.g. [[Dynamic Hashing]])