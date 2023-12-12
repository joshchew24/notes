# OLAP
On-Line Analytical Processing
- perform complex SQL queries and views
	- trend analysis
	- drilling down for more details
	- rolling up to provide more easily understood summaries
- enables complex data analysis 
- few, but complex queries, may run for hours
## Queries
- queries normally performed by domain experts rather than database experts
- full of grouping and aggregation
- imagine a **multidimensional model**
	- set of numerical **measures**
		- quantities that are important for business analysis
			- e.g. sales, etc.
	- set of **dimensions**
		- entities on which measures depend on
			- e.g. location, date, etc.
- **fact table**
	- relates dimensions to a measure using FK
## Star Schema
- fact table references dimension tables