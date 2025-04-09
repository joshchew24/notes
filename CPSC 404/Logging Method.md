---
aliases:
  - Undo/Redo Logging
---
# Logging Method
- within [[Write-Ahead Logging Protocol|WAL]] framework, could follow any logging method for [[Crash Recovery]]
## Undo
- maintain log such that to recover from a crash, just undo incomplete transactions
## Redo
- maintain log such that to recover from a crash, just redo committed transactions
## Undo/Redo
### Overview
- maintain log such that to recover from a crash, redo committed transactions **and** undo incomplete ones
	- [[LogRecord]] to track each DB element modification by a transaction
	- *Update* records **must** be written to disk **before** $T$'s changes to $X$ are written to disk
- offers most flexibility
- similar in spirit to [[ARIES Crash Recovery Framework|ARIES]]
- use [[Checkpointing]] to know from **where to start recovery**
### Steps
1. analysis: ID **incomplete** and **committed** transactions
2. redo actions of **committed** transactions (earliest first, go forward)
3. undo actions of **incomplete** transactions (latest first, go backward)
### Notation
- **three** address spaces in transaction management
	- space of disk pages
		- stores DB elements
		- stable storage
	- virtual or main memory address space
		- managed by buffer manager
	- local address space for each transaction
- `INPUT(X)`
	- copy DB element into a memory buffer
		- i.e. copy disk page containing $X$
- `READ(X, t)`
	- copy DB element $X$ to transaction's local variable $t$
	- if $X$ is not copied from disk to buffer, do it first (`INPUT`) and then do this read
- `WRITE(X, t)`
	- copy value of local variable $t$ to DB element $X$
	- if page containing $X$ not in buffer, copy it first (`INPUT`), then do the write
- `OUTPUT(X)`
	- copy DB element $X$ to DB
		- i.e. copy page containing $X$ from buffer to disk