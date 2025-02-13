# Query Optimizer (QO)
- relies on [[System Catalog]] in choosing efficient plans to evaluate queries
	- stats help estimate how **selective** certain predicates and joins are
	- **selectivity estimation (SE)** is key for QO
		- used to estimate cost of different [[Query Evaluation Plans]]
	- use of [[Histogram]]s is one of many techniques for SE
