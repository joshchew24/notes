---
aliases:
  - Group By
---
# Group By Evaluation
- e.g. $\Gamma_{\text{year}:count(distinct \text{ sname})}$
	- i.e. aggregate counts of sname, group by year
- see also [[Having Evaluation|Having]]
- if **attrs** are a **prefix** of a [[B+ Trees]] Index, can retrieve tuples in **sorted order**
	- simplifies eval, just perform linear pass
	- easy to delineate groups since sorted
	- [[Clustered Indexes|Clustered Index]] is **irrelevant**, since we only need to read DEs
		- assumes alt2
