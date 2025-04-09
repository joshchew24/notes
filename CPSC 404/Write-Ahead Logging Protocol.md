---
aliases:
  - WAL
---
# Write-Ahead Logging Protocol
## Write-Ahead Logging Protocol
1. must **ensure** the **log record** for an update is written to disk **before** the corresponding **data page** is written to disk
	- guarantees atomicity
2. must **write all log records** for a transaction **before** transaction **commits**
	- guarantees durability
## Note
- when current transaction is committed, the **log tail** is forced to stable storage, even when a **no-force** approach is used
- i.e. Force/No-force is a strategy for writing **data pages** to disk
	- WAL is a strategy for writing **log pages** to disk