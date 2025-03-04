---
aliases:
  - PNL
  - Simple Nested Loops
  - SNL
---
# Page-based Nested Loops Join
```
foreach page p in R {
	foreach page q in S {
		join all pairs of tuples in p and q and add to result
	}
}
```
- **cost**: $M + M \times N = 1000 + 1000\times500$ I/Os
	- if smaller relation ($S$) chosen as outer, **cost**: $500 + 500\times1000$ I/Os

