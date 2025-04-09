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
- **STEAL** and **NO-STEAL**
- **FORCE** and **NO-FORCE**
## Log-Based Recovery Methods
- use a [[Log]] and write any database changes first in the log
- use the log to **Undo** or **Redo** transactions
- log should **record** the **old** and **new** values for a database item
- many algorithms, popular one is [[ARIES Crash Recovery Framework]]
	- simple, log-based recovery algo
	- works well with **Steal** and **No-Force**
	- handles crashes **during** recovery