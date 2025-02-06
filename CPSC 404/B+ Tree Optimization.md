# B+ Tree Optimization
## Prefix Key Compression
- **Motivation**: increase # of keys per node
	- more data with less tree height, fewer I/Os
- Keys in IEs only *direct traffic*, can often **compress** them
	- e.g. string prefixes as keys
- insert/delete must be **suitably modified**
- **effective** for any field/attribute whose data type is a **long string** (e.g. customer code, tracking numbers)
## Bulk Loading of B+ Trees
- **Motivation**: repeated insertions would take a ton of random I/Os
- sort all data entries, then sequentially read sorted pages and construct index from left to right
- index entries for leaf pages are always entered in the right-most index page just above leaf level
	- when filled up, it splits up the right-most path to the root
- anything except the right-most path can be flushed to disk