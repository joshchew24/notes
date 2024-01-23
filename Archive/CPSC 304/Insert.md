# Insert

## Redux
- can add values selected from another table
- e.g. enroll student 51135593 into every class taught by faculty 90873519
```sql
INSERT INTO Enrolled
SELECT 51135593, Name
FROM Class
WHERE fid = 90873519
```
- SELECT-FROM-WHERE statement fully evaluated before its result is inserted into Enrolled