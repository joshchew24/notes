# 3rd Normal Form (3NF)
## Definition
A relation R is in 3NF if:
- for all $X\rightarrow b$ non-trivial dependencies in R
	- $X$ is a superkey for R
		- [[Boyce-Codd Normal Form (BCNF)|BCNF]]
	- or $b$ is part of a minimal key
## Validate Normalized Form
1. Find all keys from FDs F
2. Create [[Minimal Cover|minimal cover]] F'
3. Check if any FDs in minimal cover violate 3NF
4. If yes, decompose
## Decomposition
### Lossless Join Method
1. Given the FDs F, compute F' (minimal cover)
2. Decompose using F' if violating 3NF
	- all the way down to BCNF
3. After each decomposition, identify the set of dependencies N in F' that are not preserved by the decomp
4. At the end, for each $X->b$ in N, create a Relation $R_n(X\cup b)$ and add it to the decomposition

### Synthesis Method
1. Find minimal cover F'
2. For each FD $X\rightarrow b$ in F', add relation $R_n(X, b)$ to the decomposition for R
3. Remove any redundant relations
	- all attributes exist in another relation
4. If there are no relations in the decomposition that contain all attributes of a key, add one, preserving lossless joins