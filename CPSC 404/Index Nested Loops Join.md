---
aliases:
  - INL
---
# Index Nested Loops Join
- assumes there is a matching index of S
```
foreach tuple r in R {
	probe index of S for query value
	foreach tuple s from index probe of S {
		add <r, s> to result
	}
}
```
- **cost**: $M + M\times T_R \times \text{cost of finding matching S-tuples})$
	- $M$: # pages of $R$
	- $T_R$: # tuples of $R$
	- cost of finding matching S-tuples $\equiv$ cost of index probe
## Index Probe Cost
- commercial [[Query Optimizer (QO)]]s use heuristic to estimate cost
	- for each $R$ tuple, cost of probing $S$ **alt1** index to find [[Data Entries]]:
		- [[Hash-Based Index]]: 1.2
		- [[B+ Trees]]: 2-4
	- additional cost for **alt2**