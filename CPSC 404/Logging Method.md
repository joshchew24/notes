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
	- *Update* records **must** be written to disk **before
- offers most flexibility
- similar in spirit to [[ARIES Crash Recovery Framework|ARIES]]
- use [[Checkpointing]] to know from **where to start recovery**
- 
### Steps
1. analysis: ID **incomplete** and **committed** transactions
2. redo actions of **committed** transactions (earliest first, go forward)
3. undo actions of **incomplete** transactions (latest first, go backward)
