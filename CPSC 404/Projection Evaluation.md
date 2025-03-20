---
aliases:
  - Projection
---
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
- [[Duplicate Elimination]] is facilitated by [[Partitioning]]
	- [[Hash Join]]: eliminate within hash partitions
	- [[Sort Merge Join|SMJ]]: deduplicate SSLs, deduplicate during merge-join
## [[Duplicate Elimination]]
- given $R(A,B,C,D)$
```sql
SELECT DISTINCT A,B
FROM R
```
- using [[Sort Merge Join|SMJ]]
	- read $R$ into buffer
	- project on $A,B$
	- sort in memory
	- write SSLs to disk
	- if buffer large enough, merge all SSLs
		- eliminate any duplicates that arise
	- **cost**
		- if buffer has **at least** one more page than the # SSLs, cost is $3 \times \# \text{ pages of R}$
- using [[Hash Join|HJ]]
	- similar, eliminate duplicates within partitions
