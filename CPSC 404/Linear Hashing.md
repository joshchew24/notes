# Linear Hashing
## Bucket Split Technique
- **suffix bits** of the hash function classify the keys
- when a bucket splits, **partition** its keys according to **the next most significant bit**
- new pair of buckets are the original and its split image
- splitting policy is flexible, can use any criterion
	- e.g. whenever new overflow page is created
	- whenever any entry is added to an overflow page
	- every 3 new overflow pages
## Round Robin Splitting
- splits buckets in **rounds of splitting**
	- round $i$ of splitting begins with $2^i$ buckets
		- starts at $i=0$
	- during the round, **split each bucket**, ending the round with $2^{i+1}$ buckets
![[Pasted image 20250212205943.png]]
## Hash Functions
- uses **two hash functions per round** from a family of hash functions
- $h_{i}(k) = k \mod 2^i \text{    for } k \geq 0$
- in round $i$ of splitting, use functions $h_i$ and $h_{i+1}$ 
- $i$ is sometimes referred to as $level$
- 