# Table Scan
- read the whole table (usually from disk)
- e.g. $\sigma_{genre}$
```sql
SELECT * 
FROM Songs
WHERE genre = 'hiphop' AND year > 2010
```
- plan 1: evaluate this selection query using a simple scan of the `Songs` table
	- cost: size of `Songs` table is 500 pages, so 500 I/Os