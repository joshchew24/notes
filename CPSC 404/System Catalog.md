---
aliases:
  - Catalog Tables
  - Data Dictionary
  - Catalog
---
# System Catalog
- maintains important **metadata** about all data in the DB
- what is data?
	- **application data** in tables
	- **all indexes** for the tables
- aka catalog tables, data dictionary, catalog
## Catalog Contents
### "Static" Info
- "static" because:
	- table and index metadata is pretty stable, unlikely to be changed in the lifetime of the database
	- materialized views may not be kept in sync with base tables
- for all tables:
	- table name
	- file name
	- file structure (e.g. sorted/heap file)
	- attribute names/types
	- index names
	- primary and foreign key constraints
- index
	- index name
	- structure (hash, B+tree, clustered, search key attributes)
- auxiliary info on views
	- [[CPSC 404/Views]]
	- also see [[Archive/CPSC 304/Views]] 
	- e.g. any materialized views?
### Statistical Info
- statistics are allowed to become stale because constantly recomputing is costly
- for all relations,
	- cardinality = # tuples
	- size = # pages
	- index cardinality = # distinct keys
	- index size = # index pages 
		- for [[B+ Trees]] alt2 and alt3, index size is # leaves
	- index height - only for tree indexes
	- index range - min and max key values in the tree
- most of the static info is stored as a table
- may maintain additional info
	- e.g. [[Histogram]]s of value distribution for various attributes on which indexes have been bilt
