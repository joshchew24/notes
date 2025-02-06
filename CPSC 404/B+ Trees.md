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
## Insertion
### Procedure of inserting DE $k*$ to B+Tree $T$
- split the leaf node, copy up the middle key into the index
	- **copy** the value, keep the DE in the leaf
	- if splitting an internal node (index), just move the value up
#### Pseudocode 
```
find correct leaf L that k* belongs to
try to add k* to L
	if L not full, done
	else:
		split L into two halves (distribute entries evenly)
		copy up middle key to parent index
		if parent index full, split and move middle key up
		update parent index pointers
```
### Alternative
- merge leaf node with adjacent one
- redistribute DEs
- update root with correct discriminator value
- can help to avoid increasing tree height
## Examples
### jan 23
- RAM = 10000 pages
- root has 120 children, have 100 children each
how many **complete** levels of the B+ tree can we prefetch and keep buffered in ram?
- root = 1 page, children = 120 pages, grandchildren = 120\*100 = 12000 pages, doesn't fit in ram
- therefore we prefetch root and children
	- grandchildren maybe only prefetch based on access patterns
### Clicker Qs
1. root and 3 children ![[Pasted image 20250131173302.png]]
2. $1\times 5\text{ reads}+8\times4\text{ reads}+6\times3\text{ reads} = 55 \text{ reads}$![[Pasted image 20250131174557.png|500]]
3. $1\times4+14\times3=46$ ![[Pasted image 20250131175218.png|574]]
4. 