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
- [[Tuple-based Nested Loops Join]]
	- super naive
- [[Page-based Nested Loops Join]]
	- slight improvement over [[Tuple-based Nested Loops Join|TNL]]
- [[Index Nested Loops Join]]