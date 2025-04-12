---
aliases:
  - INL
---
 # Index-based Nested Loops Join
- assumes there is a **matching index** of $S$
	- $S$ is the **inner table**
	- $R$ is the **outer table**
```
foreach tuple r in R {
	probe index of S for query value
	foreach tuple s from index probe of S {
		add <r, s> to result
	}
}
```
- **cost**: $M + M\times T_R \times \text{cost of finding matching S-tuples}$
	- $M$: # pages of $R$
	- $T_R$: # tuples of $R$
	- cost of finding matching S-tuples $\equiv$ cost of index probe
## Index Probe Cost
- commercial [[Query Optimizer (QO)]]s use heuristic to estimate cost
	- for each $R$ tuple, cost of probing $S$ **alt1** index to find [[Data Entries]]:
		- [[Hash-Based Index]]: 1.2
		- [[B+ Trees]]: 2-4
	- additional cost for **alt2**/**alt3**:
		- [[Clustered Indexes]]: 1 I/O
		- Non-clustered: up to 1 I/O **per matching** S-tuple
## Examples
- uses [[Join Evaluation#^example-schema|Example Schema]]
### Example 1
- **alt2** hash index on $sid$ of $Songs$ (as inner)
	- scan $Ratings$: $1000$ page I/Os, $100\times1000$ tuples
	- for each $Ratings$ tuple: $1.2$ I/Os to get DE from index, plus $1$ I/O to get matching $Songs$ tuple
		- there is **exactly one** match, because $sid$ is the [[Primary Key]]
	- total: $1000 + (1.2+1)\times100\times1000=221\,000$ I/Os
### Example 2
- **alt3** hash index on $sid$ of $Ratings$ (as inner)
	- scan $Songs$: 500 page I/Os, $80 \times 500 = 40\,000$ tuples
	- for each $Songs$ tuple:
		- 1.2 I/Os to find index page with DEs
		- cost of retrieving matching $Ratings$ tuples
			- assuming uniform distribution, there are 2.5 ratings per song ($\frac{100\,000}{40\,000}$)
			- if clustered: $1$ I/O, otherwise $2.5$
	- total:
		- clustered: $500 + 40\,000\times(1.2+1) = 88\,500$ I/Os
		- unclustered: $500 + 40\,000 \times (1.2 + 2.5) = 148\,500$ I/Os