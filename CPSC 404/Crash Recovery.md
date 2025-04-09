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
## Buffer Pool Management Policies
DBMS guarantees the following:
- The changes for any transaction are [[Transaction Properties#Durability|Durable]] once the DBMS has told somebody that it committed
- No partial changes are durable if the transaction aborted.
### Steal Policy
- dictates whether the DBMS **allows** an **uncommitted transaction** to **overwrite** the most recent committed value of an object in **non-volatile storage** (can a transaction write uncommitted changes to disk)
	- **STEAL**: is allowed
		- when system *needs* the page, write to disk even if transactions that changed it are **still active**
		- i.e. if buffer is full, write dirty page to disk to free the space
		- i.e. transaction can write changes to disk before committing
	- **NO-STEAL**: is *not* allowed
		- keep a page **in memory** if the **transaction that updated it is active**
		- i.e. all pages modified by a transaction are kept in memory until committed
		- i.e. transaction can't write changes to disk before committing
### Force Policy
- dictates whether the DBMS **requires** that **all updates** made by a transaction are **reflected on non-volatile storage** **before** the transaction is **allowed to commit**
	- **FORCE**: is required
		- when a transaction wants to **commit**, write **all pages** modified by it, then commit
	- **NO-FORCE**: is *not* required
## Flushing Buffer Pool
- transactions modify pages in memory buffers
- **when** should updated pages be written back to disk?
	1. **steal** 
		- when system *needs* the page, write to disk even if transactions that changed it are **still active**
		- i.e. if buffer is full, write dirty page to disk to free the space
	2. **no-steal** 
		- keep a page **in memory** if the **transaction that updated it is active**
		- i.e. all pages modified by a transaction are kept in memory until committed
	3. **force**
		- when a transaction **commits**, write **all pages** modified by it
	4. **no-force**
		- when a transaction **commits**, no need to write all pages modified by it; pages are **only written when their buffers are needed**