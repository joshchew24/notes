# Log
- file of action reords stored in **stable storage**
	- records sequence of actions on the DB
	- **log tail**: **most recent page(s**) of log file is **in memory**
- each log record has a unique **Log Sequence Number** (LSN)
	- LSN is always increasing
- each *data page* contains a **pageLSN**
	- LSN of the **most recent** log record that made a change to that page
- log records from **same transaction** form a **back-linked list**
	- every log record has
		- **transID, prevLSN, type**
- ![[Pasted image 20250408184603.png]]
## [[LogRecord]]
- $<T_i, X, N_{old}, N_{new}>$
- $X$ is ID of DB element