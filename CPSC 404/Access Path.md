# Access Path
- method for accessing relevant tuples of a table
- e.g. [[Table Scan]], [[Index Scan]], [[Indexing|Index Probe]]
- for the most part, only consider conditions $C$ which are conjunctions of atomic conditions of the form `attr op value`
	- conjunctive selections are far more common than arbitrary boolean combinations of atomic conditions
	- e.g. $C \equiv sid=123 \land year=2016 \land genre= 'pop'$