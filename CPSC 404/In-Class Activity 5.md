# In-Class Activity 5
## Question 1
- Patient: 120 pages
- Treatment: 250 pages
- $B_1$: 10 pages
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
- $R$ has $M=100000$ pages
- $S$ has $N = 50000$ pages
- $B = 3(M+N)$
- 