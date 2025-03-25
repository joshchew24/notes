# Concurrency Control
- achieved by **interleaving** [[Transactions]]
	- actions of various transactions
	- end result must be **same** as if translations were run in **isolation**
	- if each transaction is **consistent** and executed in **isolation**, any sequence of transactions leaves DB in **consistent state**
## [[Conflicts]]