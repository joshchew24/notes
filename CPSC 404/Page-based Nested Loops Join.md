---
aliases:
  - PNL
  - Simple Nested Loops
  - SNL
---
# Page-based Nested Loops Join
```
foreach page p in R {
	foreach page q in S {
		join all pairs of tuples in p and q and add to result
	}
}
```
- **cost**: $M + M \times N = 1000 + 1000\times500$ I/Os
	- if smaller relation ($S$) chosen as outer, **cost**: $500 + 500\times1000$ I/Os
	- every page of **outer** is a random read $(M$)
	- every pass of **inner** has one random read, rest are sequential ($M$ random, $M(N-1)$ sequential)
	- total: $2M$ random, $M(N-1)$ sequential
## Example
- outer relation $R$ has **500** pages
- random access costs **15** ms (average rotational delay + average seek time)
	- total random read/write: **15.5**ms
- sequential access costs **0.5** ms (transfer time)
- for what size of the inner relation does time spent on random access exceed 20% of time spent on sequential access?
### Numerical Approach
- let $O$ = # of outer pages
- let $I$ = # of inner pages
$$\text{total I/Os} = O\times(\text{random access} + \text{transfer time}) + O \times I \times \text{transfer time} + O \times \text{random access}$$
$$\text{total I/Os} = 500 \times (15+0.5) + 500 \times I \times 0.5 + 500\times15$$
$$\text{total I/Os}$$
$$\text{random time} = 500 \times 15 \times 2$$
$$\text{sequential time} = 500 \times 0.5 + 500 \times 0.5 \times I$$
$$500\times 30 \text{ms} \geq 20\% (500\times0.5 + 500\times0.5\times I)$$
$$1500 \geq 0.2 \times 250 \times (I+1)$$
$$1500 \geq 50\times(I+1)$$
$$300 \geq I + 1$$
$$299 \geq I$$
### Formulaic Approach
- this gives a different answer because it assumes "random access" includes transfer time (15.5 ms instead of just 15)
$$2M \times 15.5 > 0.2 \times M(N-1) \times 0.5$$
$$2 \times 15.5 > 0.1 \times (N-1)$$
$$310 > N-1$$
$$310 + 1 > N$$
$$311 > N$$
#### Formula Equivalence Proof

$$\text{Total I/Os: }(M \times 15.5) + (M \times N \times 0.5) + (M \times 15)$$
$$(M\times 15.5)  + N(M\times0.5)+ (M\times 15)$$
$$(M \times 15.5) + (N-1)(M\times0.5)  + (M\times0.5) + (M \times 15) $$
$$(M\times15.5) + (N-1)(M\times0.5) + (M\times15.5)$$
$$2(M\times15.5) + (N-1)(M\times0.5)$$