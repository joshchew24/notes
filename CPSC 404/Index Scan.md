# Index Scan
- read all data entries in the index file
	- makes sense for alt2, alt3
		- **alt1 is just table scan**
		- data entries are **much shorter**, just contain the **attributes** on which the **index was built**
		- for alt2, needs to be **dense**
			- need to scan data entries for all records
			- sparse index would not contain most of the data
- e.g. given an index on `(uid, sid, time)` for `Ratings(uid, sid, time, rating)` and query:
```sql
SELECT sid
FROM Ratings
WHERE uid = u123 AND time > 2019
```
- can be evaluated by **scanning** the *index file*
	- cost of plan: # of pages of index file holding DEs