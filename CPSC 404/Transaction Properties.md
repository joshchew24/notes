---
aliases:
  - ACID
---
# Transaction Properties
DBMS ensures ACID properties:
## Atomicity
- either **all or none** of the operations are **completed**
- system **has to** recover from failures
- DBMS must **undo** aborted transactions
## Consistency
- DB state is **consistent** after transaction
- e.g. transfering from one account to another, check that sum of balances are same before/after
	- programmer is responsible for writing consistent transactions
## Isolation
- **concurrent** transactions **must not interfere with each other**
- DBMS must perform [[Concurrency Control]]
## Durability
- changes from **successful** transactions **must persist through failures**
- keep enough info on disk to recover
- user must **commit** changes to assume DB data has been changed
- DBMS must **redo** actions of **committed** transactions **after a crash**
	- [[Recovery]]
		- DBMS **logs** all actions to undo/redo
		- done by **recovery manager**