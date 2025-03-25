# Lock-based Concurrency Control
- method of [[Concurrency Control]]
- **locks**
	- structures that restrict access to DB objects
	- **shared (S)** locks
		- can be shared by multiple transactions
	- **exclusive (X)** locks
		- can be held by a single transaction at atime
	- **locking protocol** defines how locks are used