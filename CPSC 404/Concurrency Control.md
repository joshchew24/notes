# Concurrency Control
- achieved by **interleaving** [[Transactions]]
	- actions of various transactions
	- end result must be **same** as if translations were run in **isolation**
	- if each transaction is **consistent** and executed in **isolation**, any sequence of transactions leaves DB in **consistent state**
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
#### Write-Write (WW)