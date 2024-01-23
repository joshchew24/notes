# Outer Join
- include all tuples from one or both of the relations
	- for a non-matched tuple, fill the complement's attributes with NULL
![[Pasted image 20231211181122.png]]
## Left join
- include all tuples from left relation
```sql
SELECT *
FROM Student S
LEFT OUTER JOIN Enrolled E
ON S.snum = E.snum
```
- *`OUTER` keyword is optional*
![[Pasted image 20231211181135.png]]
## Right join
- include all tuples from right relation
```sql
SELECT * 
FROM Student S
RIGHT JOIN Enrolled E
ON S.snum = E.snum
```
![[Pasted image 20231211181147.png]]
## Full Join
- include all tuples from both relations
- matched tuples satisfy the ON condition
```sql
SELECT *
FROM Student S
FULL JOIN Enrolled E
On S.snum = E.snum
```
![[Pasted image 20231211181202.png]]
## Natural Outer Join
```sql
SELECT *
FROM Student S
NATURAL LEFT OUTER JOIN ENROLLED E
```
