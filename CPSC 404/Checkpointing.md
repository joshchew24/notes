# Checkpointing
## Process
1. write a `<START CKPT T1, ..., Tk>` record to log
	- $T_1,\, \dots, T_k$ are currently active transactions; 
2. immediately flush the log to disk
3. write all **dirty pages** to disk
	- may iunclude pages modified by **uncommitted transactions**
4. write an `<END CKPT>`  record to log, flush immediately