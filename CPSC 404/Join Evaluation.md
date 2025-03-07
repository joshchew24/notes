# Join Evaluation
```sql
SELECT *
FROM Ratings r, Songs s
WHERE r.uid = s.uid
```
- in algebra, $R\Join S$
- equivalent to cross-product followed by selection
	- very inefficient
	- several *direct* and *efficient* algorithms exist
> [!note] Schema for Examples
> - $Songs(sid: \text{int}, \,sname: \text{string},\,genre:\text{string},\,year:\text{date})$
> 	- tuple size: 50 bytes
> 	- page size: ~80 tuples
> 	- relation size: 500 pages
> - $Ratings(uid: \text{int},\,sid:\text{int},\,time:\text{date},\,rating:\text{int})$ 
> 	- tuple size: 40 bytes
> 	- page size: ~100 tuples
> 	- relation size: 1000 pages
> - page size: 4096 bytes (4K)
> ^example-schema
## Nested Loop Joins
- [[Tuple-based Nested Loops Join]]
	- super naive
	- for **each tuple** of $R$, check for join with **each tuple** of $S$
- [[Page-based Nested Loops Join]] (SNL)
	- slight improvement over [[Tuple-based Nested Loops Join|TNL]]
	- for **each page** of $R$, check for join with **each page** of $S$
- [[Index-based Nested Loops Join]]
	- improvement over [[Page-based Nested Loops Join|SNL]]
	- for **each tuple** of $R$, **probe $S$ index** for joinable tuples
- [[Chunk-based Nested Loops Join]]
	- improvement over [[Index-based Nested Loops Join|INL]]
	- for **each chunk** of $R$, check for join with **each page** of $S$
## Better Methods
- [[Sort Merge Join]]
- [[Hash Join]]
## Multiple Attributes
### Equality
- e.g. $R.A = S.A \land R.B = S.B$
- for [[INL]]
	- build or use existing index on $(A,B)$ for $S$
- for [[SMJ]] and [[HJ]], sort/partition on combination of the two join columns