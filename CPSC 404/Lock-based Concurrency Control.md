# Lock-based Concurrency Control
- method of [[Concurrency Control]]
- **locks**
	- structures that restrict access to DB objects
	- **shared (S)** locks
		- can be shared by multiple transactions
	- **exclusive (X)** locks
		- can be held by a single transaction at a time
		- if a [[Transactions|Transaction]] holds an **X lock** on an object, no other transaction can get **any** (S or X) lock on that object
	- **locking protocol** defines how locks are used
- **each transaction** must obtain
	- **S or X** lock on an object before **reading** it
	- **X** lock on an object before writing it
- handled by [[Lock Manager]]
## [[Two-Phase Locking Protocol]]