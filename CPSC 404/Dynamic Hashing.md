# Dynamic Hashing
## [[Extendible Hashing]]
## [[Linear Hashing]]
- alternative dynamic hashing framework to extendible hashing
- splits bucket using same technique
	- **additional suffix bit of the address** used to re-hash the bucket being split
- differs in **bucket splitting**
	- extendible hashing splits buckets **as soon as they overflow**
	- linear hashing splits buckets according to a **fixed schedule**
		- round robin splitting