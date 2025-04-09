---
aliases:
  - Recovery
---
# Crash Recovery
## Motivation
![[Pasted image 20250408171824.png]]
- if a crash occurs
	- T1, T2, T3 should be [[Transaction Properties#Durability|Durable]]
	- T4 and T5 should be aborted
		- i.e. rolled back
## Flushing Buffer Pool
- transactions modify pages in memory buffers
- **when** should updated pages be written back to disk?
### Different 
1. **steal** approach
2. **no-steal** approach
3. **force**
4. **no-force**