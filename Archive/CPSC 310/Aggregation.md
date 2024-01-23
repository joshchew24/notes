# Aggregation
## [[Group By]]
## [[Having]]

```sql
SELECT [DISTINCT] target-list
FROM relation-list
WHERE qualification
GROUP BY grouping-list
HAVING group-qualification
ORDER BY target-list
```
- each answer tuple corresponds to a *group*
	- set of tuples with same values for all attributes in `grouping-list`
	- selected attributes must have a single value per group
- `target-list` may only contain
	1. attribute names in the `grouping-list`
	2. terms with aggregate operations (e.g. `MIN (S.age)`)
	- i.e. we can only select attributes that we are either GROUPING BY or AGGREGATING ON
		- think of 310 query EBNF
- attributes in `group-qualification` are either in `grouping-list` or are arguments to an aggregate operator