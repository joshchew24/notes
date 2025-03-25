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
	- **problems**
		- if T1 **aborts** before committing change to A, T2 will use wrong data
			- why wrong?
		- 
#### Read-Write (RW)
#### Write-Write (WW)