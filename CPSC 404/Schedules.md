# Schedules
- **definition**: time-ordered sequence of the actions for one or more transactions such that
	- actions from transaction $T$ appear in schedule **in the same order** that they do in $T$
	- i.e. schedule is the sequence of interleaved transactions
## Schedule Types
### Concurrent Schedule
- actions of two or more transactions are interleaved
### Serial Schedule
- schedule does not interleave actions of different transactions
- i.e. actions of one transaction are executed together
### Equivalent Schedules
- for any DB state, effect (on set of objects in DB) of executing schedule $A$ is identical to effects of executing schedule $B$
### Serializable Schedule
- schedule equivalent to some serial schedule
- if each transaction preserves consistency, every serializable schedule preserves consistency
#### Example of (Concurrent) Non-serializable Schedule
![[Pasted image 20250324163939.png]]
- 