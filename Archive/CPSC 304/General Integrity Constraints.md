# General Integrity Constraints
`CHECK <condition>`
e.g.
```sql
CREATE TABLE Student(
	snum INT,
	sname CHAR(32),
	major CHAR(32),
	standing CHAR(2),
	age REAL,
	PRIMARY KEY (snum),
>   CHECK (age >= 10 AND age < 100)   <
)
```
- constraints can be named
- can use subqueries to express constraint
- specify constraints over a *single* table using table constraints
	- conditional expression in the check class can refer to other tables
e.g. no-one can be enrolled in a class that is held in R15
```sql
CREATE TABLE Enrolled (
	snum INT,
	cname CHAR(32),
	PRIMARY KEY (snum, cname),
	CONSTRAINT noR15
	CHECK ('R15' <> (
		SELECT c.room
		FROM class c
		WHERE c.name = cname
	))
)