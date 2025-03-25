---
aliases:
  - 2PL
  - Strict 2PL
  - Strict Two-Phase Locking Protocol
---
# Two-Phase Locking Protocol
## Strict Two Phase Locking Protocol
### Rules
- each transaction must obtain **S or X** lock on object before reading it
- each transaction must obtain **X** lock on object before writing it
- **all locks** held by a transaction are **released** when the transaction completes (commits or aborts)
### Outcomes
- only allows conflict-serializable schedules