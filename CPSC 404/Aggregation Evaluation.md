---
aliases:
  - Aggregation
---
# Aggregation Evaluation
- see also [[Aggregation]]
## Without [[Group By Evaluation|Group By]]
- generally requires [[Table Scan]]
- given [[Indexes|Index]] whose [[Search Key]] includes all attributes in `SELECT` and `WHERE` clause, may suffice to do [[Index Scan]]

example:
```sql
SELECT AVG(Salary)
FROM Employee
WHERE Rank >= 'Associate'
```
## With [[Group By Evaluation|Group By]]
- [[Sort Merge Join|SMJ]] approach:
	- sort on group-by attribute(s)
	- scan sorted relation
	- compute aggregate for each group
		- can combine sorting/aggregation step
			- I/O cost is only that of sorting
- [[Hash Join|HJ]] approach is similar
- maintain running aggregate in buffer **per group**
### Differences
- only performing the operation on a single operation
- relatively same for SMJ, just need one extra page for output buffer
- for HJ, don't need extra page for input buffer of the inner relation
	- i.e. partition size == number of partitions == $B-1$