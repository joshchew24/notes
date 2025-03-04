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
		
	}
}
```