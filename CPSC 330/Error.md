# Error
Given a model $M$, there are usually 2 categories of errors
## Training Error
- error in the training data
- $error_{training}(M)$
## Generalization Error
- error on the entire distribution $D$ of data
- $error_D(M)$
- **this** is more interesting, but we don't have access to the entire dataset (not just training data)
	- we can approximate using various methods
		- [[Data Splitting]]
		