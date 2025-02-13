# Table Scan
- read the whole table (usually from disk)
- e.g. given `Songs(sid, sname, genre, year)`, and this query $\sigma_{genre='hiphop' \land year>2010}(Songs)$
```sql
SELECT * 
FROM Songs
WHERE genre = 'hiphop' AND year > 2010
```
- evaluate this selection query using a simple scan of the `Songs` table
	- cost: size of `Songs` table is 500 pages, so 500 I/Os