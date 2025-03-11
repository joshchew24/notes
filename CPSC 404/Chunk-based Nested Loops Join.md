---
aliases:
  - CNL
---
# Chunk-based Nested Loops Join
- also called Block-based Nested Loops in textbook
	- confusing because we use block/page interchangeably
- **overview**
	- use one page as input buffer for scanning the inner relation $S$
	- use one page as output buffer
	- use rest of buffer to hold chunks of outer relation $R$
```
foreach chunk i of R {
	foreach page q of S {
		add matching tuples of i and q to result
	}
}
```
- **cost**: $\text{outer scan} + \text{\# outer chunks} \times \text{inner scan}$
	- $\text{\# outer chunks} = \lceil \frac{\text{\# outer pages}}{\text{chunk size}} \rceil$
	- a scan of a relation costs **1 I/O per page**
- **note**: it is not always cheaper to make smaller table outer
	- usually better because fewer chunks, means fewer full scans of the inner relation
## Examples
- using [[Join Evaluation#^example-schema|Example Schema]]
### Example 1
- outer: $R$
- chunk size: 100 pages
- $R$ scan: 1000 I/Os
- number of $R$ chunks: $\frac{1000}{100} = 10$
- per chunk of $R$, scan $Songs (S)$
	- $10 \times 500 = 5000$ I/Os
- total:
	- $R$ outer: $1000 + 10 \times 500 = 6000$ I/Os
	- $S$ outer: $500 + \frac{500}{100}\times1000 = 5500$ I/Os
- if buffer only has space for chunks of 90 pages, we can BP fill $\lceil\frac{1000}{90}\rceil = 12$ times
	- i.e. we need to scan $S$ 12 times
	- $R$ outer: $1000 + \lceil \frac{1000}{90} \rceil \times 500 = 7000$ I/Os
	- $S$ outer: $500 + \lceil \frac{500}{90} \rceil \times 1000 = 6500$ I/Os
