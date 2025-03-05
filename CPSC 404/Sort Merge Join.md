---
aliases:
  - SMJ
---
# Sort Merge Join
## Naive
- **overview**
	- sort $R$ and $S$ on respective join columns
		- [[Merge Sort|External Sort]]
	- scan both simultaneously in a "merge" pass, compute the join
- merge pass: **joining two sorted relations**
	- call the set of $R$-tuples with the same value of $R.i$ an $R$-group
		- same for $S$
	- have cursors pointing to first tuple of $R$ and $S$
	- repeatedly advance $R$-cursor when $R.i < S.j$, or $S-cursor$ when $S.j < R.i$
	- when $R.i = S.j$, all tuples in current $R$-group are joinable with all tuples of current $S-group$
		- compute join, add to result, advance both cursors
	- resume scanning of $R$ and $S$ and repeat
- for $M$ pages of sorted ($R$)