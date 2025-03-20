---
aliases:
  - Set Difference
---
# Set Difference Evaluation
- $R-S$ or $R\backslash S$
- naively use [[Page-based Nested Loops Join|SNL]] or [[Chunk-based Nested Loops Join|CNL]]
- use [[Sort Merge Join|SMJ]]
	- create SSLs
	- for SSLs of $R$, exclude $S$ matches from output
	- exclude matches in output
	- inclusion condition is just opposite
- [[Hash Join]] also works