# Checkpointing
## Process
1. write a `<START CKPT T1, ..., Tk>` record to log, **immediately** flush log to disk
	- $T_1,\, \dots, T_k$ are currently active transactions
2. write all **dirty pages** to disk
	- may include pages modified by **uncommitted transactions**
	- could be interleaved with other transaction actions
3. write an `<END CKPT>`  record to log, **immediately** flush log to disk