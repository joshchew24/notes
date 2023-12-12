# Assertions
- constraints over multiple relations
- different from [[General Integrity Constraints|checks]] as they are not associated with any table
- e.g. every movie star needs to start in at least one movie
```sql
CREATE ASSERTION totalEmployment
CHECK (
	NOT EXISTS (
		(Select StarID FROM MovieStar)
		EXCEPT
		(SELECT StardID FROM StarsIN)
	)
)
```
- e.g. enforce every student to be registered in at least one course
```sql
CREATE ASSERTION Checkregistry
CHECK (
	NOT EXISTS (
		(SELECT snum FROM student)
		EXCEPT
		(SELECT snum FROM enrolled)
	)
)
```
