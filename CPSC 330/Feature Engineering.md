# Feature Engineering
> [!Definition]
> The process of **transforming raw data into features** that better represent the underlying problem to the predictive models, resulting in improved model accuracy on unseen data

## Garbage In, Garbage Out
- Better features usually help more than a better model
- good features:
	- capture **most important aspects** of the problem
	- allow learning with **few examples**
	- **generalize** to new scenarios
- **trade-off** between **simple** and **expressive** features
	- **simple features** have lower scores with less risk of overfitting
	- **complicated features** have higher scores with more risk of overfitting
## Model Dependent
- the best features may depend on what model you use
- e.g.
	- for counting-based methods like decision trees, seperate relevant **groups of variable values**
		- discretization
			- partitioning and converting continuous attributes into discrete intervals
			- enables the use of continuous features in algorithms that require discrete features