# Having
- filters the results of a [[Group By|GROUP BY]] clause
	- i.e. applies a condition to aggregated data
- filters results **AFTER** they have been grouped
	- unlike [[WHERE]] clause, which filters rows before they are grouped and aggregated
### Example:
For each course with more than 1 enrolment, find the age of the youngest student who has taken this class.
*not necessary to add qualifiers to unique attributes, but can do it for clarity*
```
SELECT E.cname, MIN(S.age)
FROM Student S, Enrolled E
WHERE S.snum = E.snum
GROUP BY E.cname
HAVING COUNT(*) > 1
```
### Example 2:
For each standing, find the number of students who took a class with "System" in the title
```
SELECT S.standing, COUNT(DISTINCT s.snum) AS scount
FROM Student S, Enrolled E
WHERE S.snum = E.snum
AND E.cname like '%System%'
GROUP BY S.standing
```
What if we:
1. remove `E.cname like '%System%'` from WHERE clause
2. add a HAVING clause with this condition?
```
SELECT s.standing, COUNT(DISTINCT S.snum) AS scount
FROM Student S, Enrolled E
WHERE S.snum = E.snum
GROUP BY S.standing
HAVING E.cname like '%System%'
```
***This does not work!!***
- resultant tuples have the following schema:
	- Result(standing, scount)

| Standing | Scount |
| -------- | ------ |
| JR       | 5      |
| SR       | 2      |
| FR       | 2      |
| SO       | 2      |
- cannot apply condition E.cname to these columns