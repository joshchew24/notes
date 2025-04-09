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
## [[Buffer Pool Management Policies]]