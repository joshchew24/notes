# Pipelining
- in [[Query Evaluation Plans]], after you perform an operation, don't write the result to disk, just use it for the next operation
	- non-root intermediary nodes
- requires left-deep or right-deep plans (see [[Operator Tree#Shapes]])
- works naturally for [[Page-based Nested Loops Join|SNL]] and [[Chunk-based Nested Loops Join|CNL]]
- does not work for [[Sort Merge Join|SMJ]] and [[Hash Join|HJ]]
	- for SMJ, requires writing SSL to disk
		- inherently a **blocking operator**
	- for [[Hash Join|HJ]], requires writing partitions to disk