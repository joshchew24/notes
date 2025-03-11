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
## [[Clustered Indexes]]
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
- **sparse alt1** index
	- per page of **records** (alt1 [[Data Entries]]), there is one **index entry**
	- **REQUIRES** that the file is sorted
		- a given record **must** live in a given page found in the index
		- otherwise **no guarantee** how any of the records in the page relate to the **keyed** record