# Operator Evaluation
- SQL is compiled into (extended) relational algebra
	- ![[Pasted image 20250304175546.png]]
	- can be represented by tree structure
## General Technique (Physical Algebra)
- these physical algebra operators are used to evaluate SQL queries
- all SQL queries (i.e. extended [[Relational Algebra]]) can be efficiently processed using just these 3 physical opreators
- **[[Index Probe]]**
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
## [[Selection Evaluation]]