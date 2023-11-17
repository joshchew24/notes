---
ta: index
---
# File Representation (Index Types)
# Flat Index
### LBN to PBN
LBN is the index number
# Multi-Level Index
### LBN to PBN
*the explanation in I23.4 does this differently*
let $n$ = number of addresses able to be stored per block
let $h$ = height of index tree
let $k$ = number of levels in the index (root has k = h)
i.e. with $k=3$ , the k-indirect is the root triple indirect block. the k-index is the lookup number into the triple indirect block
### $index_{k} = \frac{LBN \bmod n^{k}}{n^{k-1}}$

e.g. 
blocksize = 16 bytes, address size = 4 bytes
$n = 4, k=3, LBN = 58$
$index_{3} = \frac{58 \bmod 4^{3}}{4^{2}} = \frac{58}{16} = 3$
$index_{2} = \frac{58 \bmod 4^{2}}{4^{1}} = \frac{10}{4} = 2$
$index_{1} = \frac{58 \bmod 4^{1}}{4^{0}} = \frac{2}{1} = 2$

# Hybrid Index

## Mapping LBN to PBN 
if LBN maps to a direct pointer, simply use LBN as lookup into index
if LBN maps to an indirect pointer:
- let $X$ = number of pointers/addresses in the index before the entry we are searching in
- when calculating indices within the appropriate tree, use $lookup = LBN - X$ 