# Projection Evaluation
```sql
SELECT SNAME, GENRE
FROM Songs
```
- can have `distinct` in `SELECT`
	- eliminates duplicate results
## [[Query Evaluation Plans]]
- with duplicates is pretty trivial, two options
	- [[Table Scan]]
	- [[Index Scan]]
		- read all DEs from index file
	- which is better?
- duplicate elimination is facilitated by [[Partitioning]]