# Query Evaluation Plans
- a query can be evaluated using several different strategies, called **plans**
	- all should produce the exact same result
- uses [[Operator Evaluation]]
- see also [[Operator Tree]]
## Cost Assessment
- to assess I/O cost of a plan, charge for reading pages (data or index), as well as writing any *intermediate* results to disk
	- **never charge for writing final result**
		- often result is returned to application or displayed to user, not written to disk
		- this value is the same for all plans, so does not affect ranking
- assess the [[Reduction Factor]]
## Plan
- [[Operator Tree]] of operations from [[CPSC 404/Relational Algebra|Relational Algebra]]
	- each operation assigned a specific algorithm/strategy
	- each operator typically implemented using a *pull* interface
		- when an operator is *pulled* for the next output tuples, it *pulls* on its inputs and computes/retrieves them
- **how do we find the best plan?**
	- what plans are considered?
	- how is the plan cost estimated?
	- **System R** approach by IBM
## Examples
uses [[Join Evaluation#^example-schema|Example Schema]]
### Example 1
```sql
SELECT S.genre
FROM Ratings R, Songs S
WHERE R.sid=S.sid AND R.uid=50 AND S.year > 2000
```
- RA Tree: ![[Pasted image 20250306112543.png]]
- Plan 0: ![[Pasted image 20250306112554.png]]
	- missed opportunities:
		- selections could have been *pushed* earlier (reduces number of tuples inputted to join)
		- no use of indexes (if available)
	- cost: just SNL, so $500 + 500\times1000$ I/Os
		- total: $500\,500$ I/Os
- Plan 1a: ![[Pasted image 20250310150610.png]]
	- push selects before join
	- **assumptions**
		- 5 buffer pages
		- 100 users
			- uniform distribution
		- ratings range from 1996-2005 (inclusive)
			- uniform distribution
	- **cost**
		- selects: table scan for each relation
			- $\sigma_{uid=50}Ratings$
				- RF: $\frac{1}{100}$
				- yields $\frac{1}{100}\times 1000=10$ tuples
				- **1000 read + 10 write** to temp1
			- $\sigma_{year > 2000}Songs$
				- RF: $\frac{2005-2000}{(2005-1996) + 1} = \frac{5}{10}$
				- yields $\frac{1}{2} \times 500=250$ tuples
				- **500 read + 250 write** to temp2
			- subtotal: $1000 + 10 + 500 + 250 = 1760$ I/Os
		- naive SMJ
			- sort temp1
				- 5 buffer pages gives 2 SSLs which can be merged in one pass
					- 10 reads + 10 writes
				- merge 2 SSLs
					- 10 reads + 10 writes
			- sort temp2
				- 5 buffer pages give 50 SSLs, cannot be merged in one pass
					- 250 reads + 250 writes
				- merge 550 SSLs
					- requires $\lceil log_4(50)\rceil = 3$ passes
					- 3 passes of 250 reads + 250 writes each
			- merge temp1 and temp2
				- 1 scan of each sorted table = 250 reads + 10 reads
			- subtotal: $(2\times(10+10)) + (4\times(250+250)) + (250+10) = 2300$ I/Os
		- **total**: $4060$ I/Os
- Plan 1b:
	- use efficient SMJ (with replacement sort)
	- **cost**
		- select cost is same: $1760$ I/Os
		- SMJ
			- sort temp1
				- $\frac{10}{2\times5} = 1$ SSL
					- 10 reads + 10 writes
			- sort temp 2
				- pass 1
					- $\frac{250}{2\times5}=25$ SSLs
						- 250 reads + 250 writes
				- **must merge until total # SSLs fits into buffer**
				- pass 2:
					- $\frac{25}{4}$