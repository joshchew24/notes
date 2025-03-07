# Access Path
- method for accessing relevant tuples of a table
- e.g. [[Table Scan]], [[Index Scan]], [[Index Probe|Index Probe]]
- for the most part, only consider conditions $C$ which are **conjunctions** ($\land$) of **atomic conditions** of the form `attr op value`
	- conjunctive selections are far more common than arbitrary boolean combinations of atomic conditions
	- e.g. $C \equiv sid=123 \land year=2016 \land genre= 'pop'$
- **most selective access path**
	- an [[Index Probe]] or [[Index Scan]] or [[Table Scan]] that is estimated to require the fewest page I/Os
## [[Index Probe|Hash Probe]]
- when a **hash index** matches a **selection condition**, fetch tuples that satisfy the condition
	- see [[Selection Condition Match]]
- when the match (between index and condition) covers some but not all conjuncts
	- read into RAM the satisfying tuples
	- check for satisfaction of remaining conjuncts
	- [[Clustered Indexes]] makes a big difference
## [[Index Probe|B+ Tree Probe]]
- 