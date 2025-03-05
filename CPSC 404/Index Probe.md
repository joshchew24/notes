---
aliases:
  - Hash Probe
  - B+ Tree Probe
  - Indexing
---
# Index Probe
- use index to home in on tuples satisfying selection condition(s)
	- best for equality selectiont
- given a hash index `(sid)` on `Songs(sid, sname, genre, year)`, and the query:
```sql
SELECT *
FROM Songs
WHERE sid = s456
```
- can be evaluated by *probing* the index
	- use the index to binary search for the DE(s) that meets the condition
	- cost: ~1.5 I/Os to reach the DE
		- 1 more if DE is alt2
		- heuristic avg for hash index
			- accounts for overflow pages, big directory that doesn't fit in RAM
		- if B+ Tree, cost depends on height
	- [[Selection Condition Match]]
## Example 1
- **what happens when selection condition matches multiple indexes?**
- two separate indexes on `{uid, sid}` and `{time}`
- query: $time \geq 3 \land uid = 1 \land sid = 2$
## Example 2
- is Index Probe always faster than [[Table Scan]]?
- $Songs(SID, SName, Genre, Year)$
	- 500 pages, 80 tuples per page, 10 distinct $Genre$s
	- $RF = \frac{1}{10}$
- $\rho_{Genre=\text{`hiphop'}(Songs)}$
- for clustered index:
	- number of I/Os: $\frac{1}{10} \times 500$ pages
		- retrieve approximately $\frac{1}{10}$ of the data pages that match the genre