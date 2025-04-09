# Buffer Pool Management Policies
DBMS guarantees the following:
- The changes for any transaction are [[Transaction Properties#Durability|Durable]] once the DBMS has told somebody that it committed
- No partial changes are durable if the transaction aborted
## Steal Policy
- dictates whether the DBMS **allows** an **uncommitted transaction** to **overwrite** the most recent committed value of an object in **non-volatile storage** (can a transaction write uncommitted changes to disk)
	- **STEAL**: is allowed
		- when system *needs* the page, write to disk even if transactions that changed it are **still active**
		- i.e. if buffer is full, write dirty page to disk to free the space
		- i.e. transaction can write changes to disk before committing
	- **NO-STEAL**: is *not* allowed
		- keep a page **in memory** if the **transaction that updated it is active**
		- i.e. all pages modified by a transaction are kept in memory until committed
		- i.e. transaction can't write changes to disk before committing
## Force Policy
- dictates whether the DBMS **requires** that **all updates** made by a transaction are **reflected on non-volatile storage** **before** the transaction is **allowed to commit**
	- **FORCE**: is required
		- when a transaction wants to **commit**, write **all pages** modified by it, then commit
	- **NO-FORCE**: is *not* required
		- when a transaction **commits**, no need to write all pages modified by it; pages are **only written when their buffers are needed**
		- i.e. transaction can commit without writing changes to disk (to reduce I/Os), only writing when the space it occupies in the buffer is needed
## Application
- **no-steal** and **force** is the easiest to implement
	- never has to **undo** changes of an aborted transaction, because they were never written to disk
	- never has to **redo** changes of a committed transaction, because they are guaranteed to be written to disk at commit time
	- **all** data that a transaction needs to modify **must** fit in memory
- most systems use **steal** and **no-force** 
	- **steal**
		- enforcing [[Transaction Properties#Atomicity|Atomicity]] is hard
			- to steal page $P$, written by active transaction $T$, must write $P$ to disk
			- what if $T$ aborts? we must remember the old value of $P$
	- **no-force**
		- enforcing [[Transaction Properties#Durability|Durability]] is hard
			- what if system crahes before a page modified by a **committed** transaction is written to disk?
			- need to keep information to do the changes when the system starts again
- **steal and force** and **no-steal and no-force** policies can exist, but are rare
	- **SF** means pages can be written early to free BP space, but must all be written again at commit
	- **NSNF** means pages pages can't be written early to free BP space, and are only freed/written after commit