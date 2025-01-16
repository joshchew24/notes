# Error
Given a model $M$, you can analyze **errors in two ways** #idk
## Training Error
- error in the training data
- $error_{training}(M)$
## Generalization Error
- error on the entire distribution $D$ of data
- $error_D(M)$
- **this** is more interesting, but we don't have access to the entire dataset
	- we can approximate the entire population using various methods
		- [[Data Splitting]]
### Data Splitting Error Types
- $E_{train}$ is training error
	- mean train error from cross-validation
- $E_{valid}$ is validation error
	- mean validation error
- $E_{test}$ is test error
- $E_{best}$ is best possible error you could get for a given problem
	- similar to generalization error, no access to full distribution to determine best possible error