---
aliases:
  - ARIES
---
# ARIES Crash Recovery Framework
## Three Phases
### Analysis
- start from some point in the log, scan forward to identify
	- all transactions that **were active**
	- all **dirty pages** in the buffer pool at the **time of the crash**
### Redo
- start from some point in the log and **repeat all actions**, up to the crash point
- restore database state before crash
### Undo
- undo the actions of transactions that did not commit
- work **backwards** in the log