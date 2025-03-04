# Selection Evaluation
## [[Reduction Factor]] Estimation
### Single Condition
#### Q1
```sql
SELECT * 
FROM SONGS S 
WHERE S.SNAME = ‘C%’ 
```
- assume values of sname are equally like to start with any letter
- RF = 1/26. Size of Q1 ≈ |Songs| × 1/26.
#### Q2
```sql
SELECT * 
FROM SONGS S 
WHERE S.YEAR > 1975
```
- assume values of year in Songs lie in a specific range, say 1970–2021, and all values are equally likely
- RF Q2 = 27/32. Size of Q2 ≈ |Songs| × 27/32.
### Multiple Conditions
- estimate RF of atomic conditions independently
- total RF is the product of the RF of the atomic conditions
#### Q3
```sql
SELECT * 
FROM SONGS S 
WHERE S.SNAME = ‘C%’ AND YEAR > 1975
```
- RF of (Q3) ≈ 1/26 × 27/32. Size of Q3 ≈ |Songs| × 1/26 × 27/32.
## General Selection Evaluation
- two approaches
### Approach 1
- find most selective [[Access Path]], retrieve tuples using it, apply any remaining conditions that didn't match index
#### Q4
$\sigma_{time<\text{8/9/04} \land rating=5 \land sid=3}(Ratings)$
- B+ Tree on $Time$ and hash on $<rating, sid>$
- evaluation options:
	- use B+ Tree index on $Time$ to retrieve tuples satisfying first condition
		- then apply $rating=5$ and $sid=3$ conditions
	- use hash index on $<rating, sid>$, then check time condition
