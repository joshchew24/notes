# Boyce-Codd Normal Form (BCNF)
## Definition
A relation R is in BCNF if:
- For all $X \rightarrow b$ non-trivial dependencies in R
	- $X$ is a superkey for R

i.e. Whenever a set of attributes of R determine another attribute, it should determine ***all*** attributes of R

### Considerations
1.  all two attribute relations are automatically in BCNF
2. Not all FDs are preserved once in BCNF

## Decomposition
### Lossless-Join Method
- if we decompose a relation R, re-joining its components should give us exactly R
	- in this case, loss refers to 'addition of spurious information'
	- ![[Pasted image 20231213181253.png]]
1. Pick any $f\in FD$ that violates BCNF
2. Decompose R into two relations: $R_1(A - b)$ & $R_2(X\cup b)$
3. Recurse on $R_1$ and $R_2$ using violating FDs until all $R_n$ are in BCNF