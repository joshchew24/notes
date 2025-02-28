# In-Class Activity 5
## Question 1
- Patient: 120 pages
- Treatment: 250 pages
- $B_1$: 10 pages
	- $\lceil \frac{10000}{1001} \rceil = 10$ SSLs
- $B_2$: 25 pages
- largest possible chunk size: $B-2$ (reserve 1 for input and output)
### (a) patient is outer
- for $B_1 = 10$: 
	- chunk size: $B_1-2 = 8$
	- number of chunks in outer relation (Patient): $\lceil \frac{120}{8} \rceil = 15$
	- number of I/Os: $15 \times 250 + 120 = 3870$ reads
- for $B_2 = 25$:
	- chunk size: $B_2 - 2  = 23$
	- number of chunks: $\lceil \frac{120}{23} \rceil = 6$
	- number of I/Os: $6 \times 250 + 120 = 1260$ reads
### (b) treatment is outer
- for $B_1 = 10$: 
	- chunk size: $B_1-2 = 8$
	- number of chunks in outer relation (Patient): $\lceil \frac{250}{8} \rceil = 32$
	- number of I/Os: $32 \times 120 + 250 = 4090$ reads
- for $B_2 = 25$:
	- chunk size: $B_2 - 2  = 23$
	- number of chunks: $\lceil \frac{250}{23} \rceil = 11$
	- number of I/Os: $11 \times 120 + 250 = 1570$ reads
## Question 2
- $R$ initially has $M_1=100000$ pages
	- $M_2 = 3\times M_1 = 300000$ pages
- $S$ initially has $N_1 = 50000$ pages
	- $N_2 = 10 \times N_1 = 500000$ pages
- $B \geq max(\sqrt{M}, \sqrt{N})$
- $B_1 \geq max(\sqrt{100000}, \sqrt{50000})$
- $B_1 \geq 316$
- $B_2 \geq max (\sqrt{300000}, \sqrt{500000})$
- $B_2 \geq 707$
- the buffer size must be approximately doubled
	- precisely, $\frac{B_2}{B_1} = \frac{707}{317} \approx 2.24$
## Question 3
Tripling the size of the relations will triple the number of partitions, while doubling the size of the buffer will halve the number of partitions. The number of partitions should increase by $\frac{3}{2} = 1.5$
## Question 4
- external merge sort: (total 10000 I/Os)
	- $2 \times 2500$ I/Os for sort phase (read and write all pages)
		- produces $\lceil \frac{2500}{101} \rceil = 25$ SSLs
	- $2 \times 2500$ I/Os for merge phase 
		- 25 SSLs can be merged in one pass, so one I/O per read or write
- duplicate elimination (total 5000 I/Os)
	- read all pages to eliminate - 2500 I/Os
	- worst case, no duplicates, write all pages - 2500 I/Os
- **Total estimated cost: 15000 I/Os**
## Question 5
- $R(A,B,C)$: 10000 pages
- $S(A,B,C)$: 25000 pages
- $B = 1001$ pages
- first sort $R$:
	- $\lceil \frac{10000}{1001} \rceil = 10$ SSLs
	- sort phase: $2 \times 10000$ pages
	- merge phase: $2 \times 10000$ pages (can merge all SSLs in one pass)
	- total $40000$ I/Os
- next sort $S$:
	- $\lceil \frac{25000}{1001} \rceil = 25$ SSLs
	- sort phase: $2 \times 25000$ pages
	- merge phase: $2 \times 25000$ pages (can merge all SSLs in one pass)
	- total $100000$ I/Os
- merge and eliminate duplicates
	- read $R$: $10000$ I/Os
	- read $S$: $25000$ I/Os
	- writing merge union
		- assume reasonable skew: ~20% overlap
			- total pages in final merge: $8000 + 25000 = 33000$ pages
	- total: $68000$ I/Os
- total: $208000$ I/Os $= 40000 + 100000 + 68000$