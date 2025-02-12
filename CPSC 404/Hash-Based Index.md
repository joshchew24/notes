# Hash-Based Index
- Type of [[Indexes]]
- best for **equality selections**
	- never designed for range search
## Concept
- given a key, hash the key and map the hash to a bucket
- bucket contains "objects of interest"
	- in this case, [[Data Entries]]
	- size is determined by developer
		- by default, each bucket is 1 page
		- cannot be partial pages
## [[Static Hashing]]
- assumes file doesn't change much or at all
- e.g. oxford dictionary (lol?)
	- updated once a year
## [[Dynamic Hashing]]
- typical for most DB applications
- e.g. medical records
	- updated frequently