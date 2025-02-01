# B+ Trees
- balanced search tree
	- all leaves are at same depth
- all relational databases uses B+ Trees
- $d$ is order of the tree
	- want to be as large as possible to have shorter trees
		- i.e. 2d should be page size
	- d <= m <= 2d keys per node
	- d + 1 pointers to 2d + 1 pointers per node
- for alt1, leaves are records
- for alt2, leaves are key/pointer pairs
## Min Height
- nodes are full (2d)
## Avg Height
- nodes are at avg fill level ($ln2 \times \text{max capacity}$)
## Max Height
- nodes are half full (d)
