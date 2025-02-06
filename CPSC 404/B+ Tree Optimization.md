# B+ Tree Optimization
## Prefix Key Compression
- **Motivation**: increase # of keys per node
	- more data with less tree height, fewer I/Os
- Keys in IEs only *direct traffic*, can often **compress** them
	- e.g. string prefixes as keys
- insert/delete must be **suitably modified**
- **effective** for any field/attribute whose data type is a **long string** (e.g. customer code, tracking numbers)
## Bulk Loading of B+ Trees
