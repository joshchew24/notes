# Selection Condition Match
- when does an index **match** a selection condition $C$? 
	- [[Hash-Based Index]] built on a set of attribute
		- i.e. **order** of attributes is **irrelevant**
		- for each [[Search Key]] attribute $A$, $C$ has a conjunct of the form $A=value$
			- e.g. A=1 AND B=2
			- invalid: A > 1 AND B = 2
				- can't range search on hash
			- valid: A = 1 AND B = 2 AND Z = 3 (assuming index not built on Z)
				- apply the Z condition to the result of the A/B probe
					- can be done in memory for no I/O cost
	- [[B+ Trees]]
		- for some prefix of search key, for every attribute $A$ in the prefix, $C$ has a conjunct of the form $A \text{ op value}$, where $\text{op} \in =, >, \geq, <, \leq$
			- search key is a sequence of attributes
				- prefix is from left to right of attribute significance
					- e.g. search key is {uid, sid}
						- valid prefixes are `uid`, and `uid, sid`
		- e.g. `Ratings(uid, sid, time)` with index `uid, sid`
			- valid conditions:
				- $uid \geq 123 \land sid = 5$
				- $sid = 5 \land uid \geq 123$
					- **order doesn't matter for the condition, even though it's important for constructing the tree**
				- $uid \geq 123 \land time \leq 2022$
					- `uid` is a valid prefix of the search key
					- `time` is not an extension of the prefix, treated as an additional conjunct
			- invalid:
				- $sid \geq 345 \land time \geq 1999$
					- `sid` is not a valid prefix of the search key (needs `uid`)
## Partial vs Total Match
### Example
- $student(SID, SName, Major, Year)$
- hash index on $\{SID\}$
- B+ tree index on ($Major, Year$)
#### Partial Matches
- $SID  = \text{`123456789'} \land Year = 3$
	- hash index
- $Major = \text{`CS'} \land SName = \text{`Jane Smith'}$
	- B+ Tree prefix
#### Invalid
- $Year > 1 \land SName = \text{`Jane Smith'}$
	- B+ tree no prefix, hash doesn't match
- $SID \geq \text{`123456789'} \land SID \leq \text{`374926338'}$
	- hash can't range search, B+ Tree search key doesn't match
#### Total
- $Year > 2 \land Major = \text{`soci'}$
