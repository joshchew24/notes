# Concurrency Control
- achieved by **interleaving** [[Transactions]]
	- actions of various transactions
	- end result must be **same** as if translations were run in **isolation**
	- if each transaction is **consistent** and executed in **isolation**, any sequence of transactions leaves DB in **consistent state**
## Conflicts
- two actions conflict if:
	- both **act on same data**
	- one of them is a **write**
### Types
#### Write-Read (WR)
- **"dirty reads"**
- e.g.
	- transaction T1 writes data A
	- transaction T2 reads and uses data A which are written by T1 before T1 is committed
#### Read-Write (RW)
#### Write-Write (WW)