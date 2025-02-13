# Relational Algebra
## Example Schema
Movie(<u>MovieID</u>, Title, Year)
StarsIn(<u>MovieID</u>, <u>StarID</u>, Character)
MovieStar(<u>StarID</u>, Name, Gender)

## Selection ($\sigma$)
### Notation
- $\sigma_p(r)$
- $p$: selection predicate
### Definition
#### Connectives
And $\wedge$, Or $\vee$, Not $\neg$
#### Predicates
- \<attribute\>$op$\<attribute\> 
- \<attribute\>$op$\<constant\>
- $op$ is one of $=, \neq, >, \geq, <, \leq$


## Division
### Definition
***A/B contains all x tuples such that for every y tuple in B, there is an x,y tuple in A***

## Least/Most
- what songs were performed in shows with the most attendance?
- pseudo-soln
	- First, find the shows that had an attendance number lower than another attendance number
	- Then, subtract these showIDs from the total list of showIDs
		- this gives us all the showIDs that did not have an attendance number lower than any other
- cannot simply find all shows that had attendance number that was higher than another, because this is not necessarily the HIGHEST
## Rename ($\rho$)
rename results of relational-algebra expressions
### Notation
$\rho(X,E)$

Returns the expression E under the name X
### Partial
we can rename part of an expression using attribute names or their position

e.g. $\rho((StarID\rightarrow ID), \pi_{StarID, Name}(MovieStar))$
e.g. $\rho((1\rightarrow ID), \pi_{StarID, Name}(MovieStar))$
## Useful Links
[Playground](https://dbis-uibk.github.io/relax/)