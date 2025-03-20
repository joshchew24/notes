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
- sort on group-by attribute(s)
	- scan sorted relation
	- compute aggregate for each group
		- can combine sorting/aggregation step
		- 