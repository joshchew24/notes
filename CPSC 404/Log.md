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
## LogRecord
- most of these details are abstracted for simpler analysis
### Fields
- `prevLSN`
- `transID`
- `type`
- `pageID`$^{+*}$
- `length`
- `offset`
- `before-image`
- `after-image`