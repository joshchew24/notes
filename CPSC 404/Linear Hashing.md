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
	- aka **level**
- **next** - pointer to next bucket to be split
	- aka **victim**
- round $R$ starts with $N_R$ buckets 
	- $0, \dots, (N_R -1)$
- round $R$ ends when all $N_R$ starting buckets have been split
	- i.e. when number of buckets is $2 \times N_R$
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
	- i.e. if 
	- i.e. if a key maps to a bucket that has already been split, we need to check another bit of the suffix to see if it should go to the split image