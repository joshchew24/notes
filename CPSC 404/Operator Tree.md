# Operator Tree
## Tree
![[Pasted image 20250306192851.png]]
## Plan
![[Pasted image 20250306192906.png]]
- **on-the-fly** means op is processed once tuples arrive in RAM thanks to processing a previous op, down the tree
	- i.e. as you read tuples for another operation, apply a project/select op
### Shapes
![[Pasted image 20250306202749.png]]
- left-deep
	- right children of internal nodes are always **base tables**
	- supports pipelining
- right-deep
	- vice versa
	- supports pipelining
- bushy
	- bad for pipelining
	- intermediary results must be written to disk