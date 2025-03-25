---
aliases:
  - Conflicting
---
# Conflicts
## Conflicts
- two actions conflict if:
	- both **act on same data**
	- one of them is a **write**
- not inherently bad, just impose **constraints** on **order of transactions** in possible equivalent serial schedules
### Types
#### Write-Read (WR)
![[Pasted image 20250324174409.png]]
- **"dirty reads"**
	- i.e. **reading uncommitted data**
- this conflict is [[Conflict Serializable Schedule|Conflict Serializable]]
- conflict
	- transaction T1 writes data A
	- transaction T2 reads and uses data A which are written by T1 before T1 is committed
- **problem**
	- if T1 **aborts** before committing change to A, T2 will use wrong data
		- value read by T2 is not correct, because it was changed by T1 which was not committed
		- T2 **must** be aborted as well
			- [[Cascaded Aborts]]
#### Read-Write (RW)
![[Pasted image 20250324175840.png]]
- **"unrepeatable reads"**
	- **transaction requests to read an entity for which an unclosed transaction has already made a write request**
- conflict
	- T1 reads A and does some calculations
	- T2 changes value of A
	- T1 writes back value from its calculation, changing A's value
- **problem**
	- value of A is modified by T2 after T1 reads it, and before it writes A
	- if T1 reads A again before writing, it will get a different value
	- not **serializable**
		- T1 expects to read/write on original value, but A gets changed miway
		- T2 expects to 
- [Wikipedia Example](https://en.wikipedia.org/wiki/Read%E2%80%93write_conflict)
	- $$ S =
\begin{bmatrix}  
T1 & T2 \\  
R(A) &  \\
 & R(A) \\
 & W(A) \\
 & Com. \\
 R(A) & \\
 W(A) & \\
 Com. & \\
\end{bmatrix}
$$
	- T1 has read the original value of A, and is waiting for T2 to finish. T2 also reads the original value of A, overwrites A, and commits.
	- However, when T1 reads from A, it discovers two different versions of A, and T1 would be forced to abort, because T1 would not know what to do
	- Alice and Bob are using a website to book tickets for a specific show. Only one ticket is left for the specific show. Alice signs on first to see that only one ticket is left, and finds it expensive. Alice takes time to decide. Bob signs on and also finds one ticket left, and orders it instantly. Bob purchases and logs off. Alice decides to buy a ticket, to find there are no tickets. This is a typical read–write conflict situation.
#### Write-Write (WW)
![[Pasted image 20250324185026.png]]
- **"overwriting uncommitted data"**
	- **transaction requests to write an entity for which an unclosed transaction has already made a write request**
- **blind writes**
	- does not depend on any existing DB object values
	- i.e. no reads in the transaction
- conflict
	- T1 writes A
	- T2 writes A before T1 commits
- **problem**
	- T1 and T2 mutually overwrite each other
		- T1 always writes B, T2 always writes A
	- e.g. T1 sets interest rate of all loans to 10%, T2 sets to 5%
		- loan A set to 5%
		- loan B set to 10%