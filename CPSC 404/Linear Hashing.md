# Linear Hashing
## Bucket Split Technique
- **suffix bits** of the hash function classify the keys
- when a bucket splits, **partition** its keys according to **the next most significant bit**
- new pair of buckets are the original and its split image
- splitting policy is flexible, can use any criterion
	- e.g. whenever new overflow page is created
	- whenever any entry is added to an overflow page
	- every 3 new overflow pages
- $d_R$ is the number of bits used for bucket indices for round $R$
## Round Robin Splitting
- splits buckets in **rounds of splitting**
	- round $i$ of splitting begins with $2^i$ buckets
		- starts at $i=0$
	- during the round, **split each bucket**, ending the round with $2^{i+1}$ buckets
	- aka **level**
- **next** - pointer to next bucket to be split
	- aka **victim**
- round $R$ starts with $N_R$ buckets 
	- $0, \dots, (N_R -1)$
- round $R$ ends when all $N_R$ starting buckets have been split
	- i.e. when number of buckets is $2 \times N_R$
	- often, $N_R = 2^R$
- in general,
	- red zone: $[0, \text{Next}-1]$ 
		- has already been split this round
	- green zone: $[\text{Next}, N_R - 1]$
		- have yet to be split this round
	- blue zone: $[N_R, N_R + \text{Next} - 1]$
		- the split images of buckets split this round
- this image assumes round $i=0$ started with $2^0$ buckets, and doubles each round![[Pasted image 20250212205943.png]]
## Hash Functions
- uses **two hash functions per round** from a family of hash functions
- $h_{i}(k) = k \mod 2^i \text{    for } k \geq 0$
- in round $i$ of splitting, use functions $h_i$ and $h_{i+1}$ 
## Insertion
- if $h_i(k)$ is less than the index of the **next** pointer, need to check $h_{i+1}(k)$
	- i.e. if key hashes into red zone, check if it should go in blue zone
	- i.e. if a key maps to a bucket that has already been split, we need to check another bit of the suffix to see if it should go to the split image
	- i.e. if key hashes into green zone, it belongs there
- if bucket is full, add the DE to an overflow page
	- check split policy
- since buckets are split round-robin, overly long overflow chains do not develop
	- every bucket eventually gets redistributed, including all of its overflow pages
	- subsequent insertions that would have collided are then distributed between the original and split image buckets
## Search
- same process as disambiguating the index to place a DE in the **Insertion** algorithm
	- can't just skip to the $h_{i+1}(k)$ function because it might give an address for a split image bucket that hasn't been created yet
## Deletion
- inverse of insertion
- if **last bucket is empty**, remove it and decrement **Next** index
	- i.e. **merge the split image with its original bucket**
	- i.e. call last bucket index $L$. move **Next** to the $(L\mod N_R) - 1$ bucket where $N_R$ is the number of buckets at the start of the round
		- $N_R=2^{d_R}$
	- **ONLY** the **LAST BUCKET**
		- to preserve the round robin order
- could also merge even if last bucket is **not empty**
	- e.g. merge whenever deletion causes last bucket to be $\leq 10\%$ full
- if **Next** points to bucket 0, and deletion leaves last bucket empty, what does it mean to decrement **Next**?
	- this means we are at the start of a round, so there are currently $N_R$ buckets
	- **Next** should point to bucket $\frac{N_R}{2}$
	- i.e. go to the end of the previous round