# Data Entries
1. alt1: whole record
2. alt2: <key, rid> pair
	1. if not a clustering index, data will be separated in the disk drive
3. alt3: <k, list of rids for search key k> (typically secondary key)
- for a key value $k$, its corresponding data entry (rid) is $k*$
- key may be primary or secondary
	- clustering index and non-clustering index