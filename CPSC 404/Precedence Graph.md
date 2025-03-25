---
aliases:
  - Serializability Graph
---
# Precedence Graph
- one node per transaction
- edge from $T_i$ to $T_j$ if 
	- $T_i$ executes W(A) **before** $T_j$ executes R(A) or W(A)
	- $T_i$ executes R(A) **before** $T_j$ executes W(A)
- **theorem**: schedule is [[Conflict Serializable Schedule|Conflict Serializable]] if its precedence graph is **acyclic**
	- topological sort over graph produces equivalent serial schedule
## Example
### Schedule
$$
S = \begin{bmatrix}
T1 & T2 & T3 \\
R(A) & & \\
W(A) & & \\
& R(B) & \\
& W(B) & \\
& & W(A) \\
& R(A) & \\
& W(A) & \\
& & W(C) \\
R(C) & & \\
W(C) & & \\
\end{bmatrix}
$$
### Precedence Graph
```mermaid
flowchart LR
	T1((T1))
	T2((T2))
	T3((T3))
	T1 --> T2
	T3 --> T2
	T1 --> T3
	T3 --> T1
```
- cycle in graph reveals problem
	- output of T1 on C depends on T3
	- output of T3 on A overrides T1
- **not conflict serializable**