---
aliases:
  - Union
---
# Union Evaluation
- e.g. $R(A,B,C) \cup S(A,B,C)$
## [[Duplicate Elimination]]
- for [[Sort Merge Join|Sort Merge]], if buffer has at least one more page than total combined # of SSLs
	- i.e. if all SSLs can be merged in one pass
	- **cost** is $3 \times (|R| + |S|)$
- for [[Hash Join|Hash]]
	- let $R$ be smaller than $S$
	- if $\frac{|R|}{B-1} \leq B-2$, then cost is $3 \times (|R| + |S|)$