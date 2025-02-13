---
aliases:
  - Index Probe
---
# Indexing
- use index to home in on tuples satisfying selection condition(s)
	- best for equality selection
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