# Checkpointing
## Process
1. write a `<START CKPT T1, ..., Tk>` record to log, **immediately** flush log to disk
	- $T_1,\, \dots, T_k$ are currently active transactions
2. write all **dirty pages** to disk
	- may include pages modified by **uncommitted transactions**
	- could be interleaved with other transaction actions
3. write an `<END CKPT>`  record to log, **immediately** flush log to disk
## Notes
- can't just have an atomic `START CKPT` operation
	- i.e. immediately flush log and dirty buffer pages
	- if crash occurs right after `START CKPT`, no guarantee that buffer flushed correctly
		- how do you use this log to recover?
- if crash during checkpoint interval, checkpoint is incomplete and should be ignored
- **start recovery** from after `<START CKPT>`
	- i.e. some transactions can still be **active** after recovery
	- **dirty pages** may be flushed **before** all actions in the checkpoint interval **complete**
		- only **guaranteed** that actions **before** the `START CKPT` were **flushed to disk**