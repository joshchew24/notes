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
- Plan 1: ![[Pasted image 20250310150610.png]]
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
		- join