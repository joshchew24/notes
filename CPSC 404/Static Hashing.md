# Static Hashing
- hash function gives address of disk page 
	- that potentially contains entry of interest
- if data distribution is imbalanced, many keys could hash to the same bucket
	- not enough room
	- solution: overflow pages