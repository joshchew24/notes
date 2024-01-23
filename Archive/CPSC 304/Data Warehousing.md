# Data Warehousing
- take raw data from multiple sources
- Extract, Transform, Load (ETL) the data into something more uniform to reflect the enterprise as understood by the user
- consolidate and integrate operational OLTP databases from many sources into one large, well-organized repository
## Learning Goals
- compare and contrast OLAP and OLTP processing
	- e.g. focus, clients, amount of data, abstraction levels, concurrency, and accuracy
- explain the ETL tasks for data warehouses
	- extract transform and load
- explain the differences between a star schema design and a snowflake design for a data warehouse, including potential tradeoffs in performance
- argue for the value of a data cube in terms of
	- the type of data in the cube (numerical, categorical, temporal, counts, sums)
	- the goals of OLAP (e.g. summarization, abstractions)
	- the operations that can be performed (drill-down, roll-up, slicing/dicing)
- estimate the complexity of a data cube, in terms of the number of eqivalent aggregation queries