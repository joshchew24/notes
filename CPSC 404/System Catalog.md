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
### Static Info
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

