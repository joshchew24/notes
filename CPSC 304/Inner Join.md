# Inner Join
## SQL
- default join behaviour
- only include matches
### Implicit Join
```sql
SELECT *
FROM Student S, Enrolled E
WHERE s.snum = e.snum
```
### Explicit Join
```sql
SELECT *
FROM Student S
INNER JOIN Enrolled E
ON S.snum = E.snum
```
- *`INNER` keyword is optional, could just use `JOIN` by itself*