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

## Useful Links
[Playground](https://dbis-uibk.github.io/relax/)