---
aliases:
  - TNL
---
# Tuple-based Nested Loops Join
```
foreach tuple r in R {
	foreach tuple s in S {
		if r.Ai == s.Bj then add <r,s> to result;
	}
}
```
- for **each tuple** in the **outer** relation $R$, scan the **entire inner** relation $S$
- **cost**: $M + T_R \times M \times N = 1000 + 100 \times 1000 \times 500 = 50001000$ I/Os