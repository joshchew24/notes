# Selection Condition Match
- when does an index **match** a selection condition $C$? 
	- [[Hash-Based Index]] built on a set of attribute
		- for each [[Search Key]] attribute $A$, $C$ has a conjunct of the form $A=value$
			- e.g. A=1 AND B=2
			- invalid: A > 1 AND B = 2
			- invalid: A = 1 AND B = 2 AND Z = 3 (assuming index not built on Z)
	- [[B+ Trees]]
		- for some prefix of search key, for every attribute $A$ in the prefix, $C$ has a conjunct of the form $A \text{op value$
		- 