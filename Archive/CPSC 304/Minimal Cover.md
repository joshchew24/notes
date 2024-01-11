# Minimal Cover
## Process
1. Put FDs in standard form (minimize RHS)
2. Minimize LHS
	- for each FD
		- for each LHS attribute
			- check the closure of the LHS without that attribute
			- if the RHS remains the same, the attribute can be removed from the FD
3. Delete Redundant FDs
	- for each FD
		- check closure of the LHS without using the FD
		- if we can still obtain the RHS, this FD is redundant