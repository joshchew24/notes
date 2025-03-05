---
aliases:
  - SMJ
---
# Sort Merge Join
## Naive Version
### Overview
- sort $R$ and $S$ on respective join columns
	- [[Merge Sort|External Sort]]
- scan both simultaneously in a "merge" pass, compute the join
### Merge Pass: joining two sorted relations
- call the set of $R$-tuples with the same value of $R.i$ an $R$-group
	- same for $S$
- have cursors pointing to first tuple of $R$ and $S$
- repeatedly advance $R$-cursor when $R.i < S.j$, or $S-cursor$ when $S.j < R.i$
- when $R.i = S.j$, all tuples in current $R$-group are joinable with all tuples of current $S-group$
	- compute join, add to result, advance both cursors
- resume scanning of $R$ and $S$ and repeat
### Cost
- for $M$ pages of (sorted) $R$ and $N$ pages of (sorted) $S$, **cost of merge-join** is $\approx M + N$ I/Os
- **overall cost**: $\text{cost of sorting } S + \text{cost of sorting } R+ M + N$
	- $(2M + 2M) + (2N + 2N) + M + N = 5(M+N)$ I/Os
- **sorting costs for each relation**
	- if $B$ has enough pages to sort each relation in one merge pass
		- cost of sorting $R = 2M + 2M$ I/Os
			- one read/write pass for sort
			- one read/write pass for merge
	- else, add $2M$ per additional pass required in the sorting phase
## Efficient Version