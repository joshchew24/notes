# B+ Trees
- balanced search tree
	- all leaves are at same depth
	- 
- all relational databases uses B+ Trees
- $d$ is order of the tree
	- want to be as large as possible to have shorter trees
	- d <= m <= 2d keys per node
	- d + 1 pointers to 2d + 1 pointers per node
## Min Height
- nodes are half full (d)
## Avg Height
- nodes are at avg fill level ($ln2 \times \text{max capacity}$)