# Static Hashing
- hash function gives address of disk page 
	- that potentially contains entry of interest
- **main limitation**: if data distribution is imbalanced, many keys could hash to the same bucket
	- not enough room
	- solution: overflow pages
## Overflow Pages
- number of primary pages (buckets) is fixed and allocated sequentially
	- at the time of building the index
		- i.e. complete control of their physical location on disk
	- if primary page is full, allocate a new overflow page
		- allocated dynamically, no control over physical location
			- i.e. more random seeks
- can chain multiple overflow pages together for a single bucket
	- long overflow chains can develop and degrade performance
## Hash Function
- $h(key)$ gives the bucket number containing $key*$ (the DE)
- $M$ is number of buckets
- must distribute values over range $0, M-1$
- $h(k)\mod M$  works well for prime $M$
- lots known about how to tune $h$
- ![[Pasted image 20250212144803.png]]
- optimize using [[Extendible Hashing]]