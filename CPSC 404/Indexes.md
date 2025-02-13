# Indexes
- think of index in a book
	- helps us quickly find something we're looking for
- id in index maps to a record ID in the actual database
- [[Search Key]] is the attribute or set/sequence of attributes on which an index is built
## Tree-structured indexing
- equality search
	- point query
	- specific
- range search
- nodes == pages
- leaf nodes/pages will store [[Data Entries]]
	- can be **alt1, alt2, alt3**
- if index file is large, make an index of index files
	- multiple layers give us a tree-structured index
## Clustered Index
- if data records are physically **sorted** on indexed attr A, then A has a clustered index
	- e.g. data is sorted on income
		- search for specific income N, all matching records are physically next to each other on disk
- technically, only needed to support range queries on primary key **?**
- can sort on any key, but a clustered index can only exist for one key at a time
	- unless the data is replicated
- given file sorted on A (not sorted on B), and B is a candidate key, can we have an unclustered index on B?
	- if only equality search, then index clustering doesn't matter
	- if we want range search, then B can have an unclustered index, but it's much slower than clustered
- pointers in leaf pages will point to sequential pages on disk
## Density
- can be **Sparse** or **Dense**
- e.g. consider *students*(**ID, name, addr, dept**) file sorted on **ID**
- sparse: alt2 index on **ID**
	- since file is sorted, only need **one index entry per block/page**
	- considered a **primary index**
	- binary search lookup
	- index is small
- dense: alt2 index on **dept**	
	- need an **index entry for every record** because we cannot exploit any ordering
	- dense because the file is sorted on ID, so cannot spare any index entries to exploit "ranges" 
	- secondary index
	- direct lookup
	- index is large (1 IE per record)