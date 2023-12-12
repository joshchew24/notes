# Group By
- all non-aggregated columns in SELECT must be in GROUP-BY
### Example 1:
For each class, find the age of the youngest student who was enrolled in this class.
```sql
SELECT cname, MIN(age)
FROM Student S, Enrolled E
WHERE S.snum = E.snum
GROUP BY cname
```
## Example 2:
Find the average age for each standing (e.g. Freshman)
```sql
SELECT standing, AVG(age)
FROM Student
GROUP BY standing
```
### Example 3:
Find the deptID and # of faculty members for each department having department ID > 20
```sql
SELECT deptID, COUNT(*)
FROM Faculty F
GROUP BY deptID
HAVING deptID > 20
```
```sql
SELECT deptID, COUNT(*)
FROM Faculty F
WHERE deptID > 20
GROUP BY deptID
```
