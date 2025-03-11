---
aliases:
  - Clustered Index
---
# Clustered Indexes
- **rows are stored physically on disk in the same order as the index**
	- for [[B+ Trees]], this means sorted
	- for [[Hash-Based Index]], this could mean partitions are stored together
		- equivalent records are stored together
- an **alt1** index **cannot** be **unclustered**
	- the data entries are the records
	- the index is physically stored together, therefore the DEs **must** be physically together, making it clustered
## Old notes
- if data records are physically **sorted** on indexed attr A, then A has a clustered index
	- e.g. data is sorted on income
		- search for specific income N, all matching records are physically next to each other on disk
- needed to support range queries on primary key 
	- also provide **faster lookup** on clustering attribute
- can sort on any key, but a clustered index can only exist for one key at a time
	- unless the data is replicated
- given file sorted on A (not sorted on B), and B is a candidate key, can we have an unclustered index on B?
	- if only equality search, then index clustering doesn't matter
	- if we want range search, then B can have an unclustered index, but it's much slower than clustered
- pointers in leaf pages will point to sequential pages on disk
- for [[Hash-Based Index]]
	- a **clustered** hash index means the equivalent records are physically stored together
