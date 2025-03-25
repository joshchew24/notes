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
- **all locks** held by a transaction are only **released** when the transaction completes (commits or aborts)
	- **omitted** by 'non-strict' 2PL
### Outcomes
- only allows conflict-serializable schedules
- avoids [[Cascaded Aborts]]
- transactions are ordered by the order they commit or abort
	- i.e. determines equivalent serial schedule
## Non-strict (regular)
### Rules
- same locking rules (S/X for read, X for write)
- a transaction can **release locks at any time**, but **cannot request any additional locks** once it releases **any lock**
	- i.e. growth phase of requesting/acquiring locks, shrink phase of shedding/releasing locks
	- **upgrades/downgrades** can occur during **growing** phase
		- a **downgrade** causes the transaction enter **shrinking** phase
### Outcomes
- only allows conflict-serializable schedules
- transactions are ordered by the order of their **entering the shrinking phase**
	- determines equivalent serial schedule
- can [[Deadlock]]
## Example
$$
s = \begin{bmatrix}
T1 & T2 \\
R(A) & \\
W(A) & \\
& R(A) \\
& W(A) \\
& Com. \\
R(B) & \\
W(B) & \\
Com.
\end{bmatrix}
$$
- strict 2PL doesn't allow this schedule, because T1 holds an X lock on A and can't release until committing, so T2 won't be able to obtain the lock
	- if T2 instead targeted object $C$, the **equivalent serial schedule** would be $T2; T1$ because of the order of commits
- regular 2PL schedule with locks shown
- $$
s = \begin{bmatrix}
T1 & T2 \\
X(A), X(B) & \\
R(A) & \\
W(A) & \\
U(A) & X(A) \\
& R(A) \\
& W(A) \\
& Com. \\
& U(A) \\
R(B) & \\
W(B) & \\
Com. & \\
U(B) & \\
\end{bmatrix}
$$
	- **equivalent serial schedule** is $T1; T2$ because of order of entering **shrinking phase**