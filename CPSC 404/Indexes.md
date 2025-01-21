# Indexes
- think of index in a book
	- helps us quickly find something we're looking for
- id in index maps to a record ID in the actual database
## Tree-structured indexing
- equality search
	- point query
	- specific
- range search
## Clustered Index
- if data records are physically **sorted** on indexed attr A, then A has a clustered index
	- e.g. data is sorted on income
		- search for specific income N, all matching records are physically next to each other on disk
- technically, only needed to support range queries on primary key