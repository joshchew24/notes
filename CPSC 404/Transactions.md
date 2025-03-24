# Transactions
- **definition**: a sequence of reads and writes that are expected to be **executed as a unit** 
	- i.e. all or nothing
	- e.g. transfer money from one account to another at bank's ATM, make travel booking, book concert tickets, etc.
	- allows for **rollback** [[Recovery]]
- DBMS abstracts state changes in **transactions**
	- reads/writes
- not concerned about the underlying **logic**
	- e.g. what value is being written, who is reading
	- i.e. **how** data was modified
- why?
	- schedules transactions by **interleaving**
	- more efficient resource usage
		- disk access is **slow**
	- simplifies reasoning about **correctness**
## Transaction States
A transaction can be in one of the following states:
### Active
- makes progress or waits for resources
### Failed
- normal execution cannot continue
- may occur because of
	- run-time error
	- system crash
	- user cancellation
- eventually **aborted**
### Aborted
- after rolling back DB to state prior to the execution of the transaction
- typically encountered in crash recovery
- **two options**
	- restart it as a new transaction later (e.g. system failures)
	- kill it (e.g. internal logical errors)
### Committed
- **guarantees** that the transaction's changes on the data/disk are **persistent**
- after successful completion of a "commit" command
- to undo effects of a committed transaction, must run a compensating transaction
- supposed to have run to completion, **logically speaking**
	- not executed yet, the DBMS will do what it takes to make sure it is
## Transaction Properties ([[Transaction Properties|ACID]])