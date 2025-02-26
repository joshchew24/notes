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
- **constraint**: left subtrees are strictly less than the parent key, right subtress are geq than the parent key
## Fill Level
- for leaves:
	- different from $d$ if using alt1 index
		- DEs can be different size than IEs (index entries - keys and pointers)
- **Rich**: if >50% fill level
- **Poor**: if 50% fill level
- **Sparse**: if <50% fill level
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
- update root with correct discriminator value/seperator entry
- can help to avoid increasing tree height
## Deletion
- start at root, find leaf $L$ where DE $k*$ belongs
- remove DE $k*$
	- if $L$ at least half full, done
	- if $L$ is less than half full, 
		- try to **re-distribute**, borrowing from siblings (adjacent node with same parent as $L$)
		- if re-distribution fails, **merge** $L$ and sibling
			- **delete** seperator entry from parent of $L$ that discriminates $L$ and sibling
			- merge could propagate to root, **decreasing height**
				- if internal node $P$ has <$d$ IEs, list all affected keys (sibling and parent) and merge
					- if even number of keys, arbitrarily select a middle. by convention, left subtree will have less members than right subturee
## [[B+ Tree Optimization]]

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