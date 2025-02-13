# Extendible Hashing
## Motivation
- primary buckets are full, want to minimize overflow
- naive idea: any time there is overflow, double # of buckets and rehash all entries
	- very expensive and inefficient
## Fundamental
- use **directory of poiners** to buckets
- directory has a **Global Depth (GD)**
	- number of least significant bits of a key required to find 