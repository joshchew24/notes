# Partitioning
- can be sort-based or hash-based
	- [[Sort Merge Join|SMJ]] and [[Hash Join|HJ]]
- useful for [[Projection Evaluation|Projection]] with duplicate elimination
- useful for [[Union Evaluation]]
	- union of two tables can produce duplicates
	- eliminate duplicates during SSL merge phase
- useful for [[Aggregation]]
	- e.g. `SELECT A,B, sum(C) FROM R GROUP BY A,B`
	- merge-aggregate SSLs 