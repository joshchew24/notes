# Natural Join

## SQL
- type of EQUI JOIN
- columns with same name of associate tables will appear once only
### Guidelines
- associated tables have at least one pair of identically named columns
	- if no pair, return the cross product of the two tables
- columns are same data type
- don't use `ON` clause
- e.g.
```sql
SELECT *
FROM Student S NATURAL JOIN Enrolled E
```