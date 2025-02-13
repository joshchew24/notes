# Extendible Hashing
https://www.youtube.com/watch?v=r4GkXtH1la8
## Motivation
- primary buckets are full, want to minimize overflow
- naive idea: any time there is overflow, double # of buckets and rehash all entries
	- very expensive and inefficient
## Fundamental
- use **directory of poiners** to buckets
- directory has a **Global Depth (GD)**
	- number of least significant bits of a key required to find corresponding bucket
	- min # of bits needed to tell which bucket an arbitrary DE belongs to
- buckets have **Local Depth (LD)**
	- number of least significant bits shared by keys within a bucket
	- min # of bits used to determine if a DE belongs to the bucket
	- if LD < GD, multiple directory entries can point to the same bucket
- Splitting Buckets
	- when a bucket overflows, the **LD increases**, and the bucket is split
	- the directory may also double in size if needed (**GD increases by 1 bit**)
- ![[Pasted image 20250212162231.png]]
	- 