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
- for counting-based methods like decision trees, seperate relevant **groups of variable values**
	- discretization
		- partitioning and converting continuous attributes into discrete intervals
		- enables the use of continuous features in algorithms that require discrete features
- for distance-based methods like kNN, we want different class labels to be "far"
	- standardization
		- avoid dominance of wide-ranging features over other features with smaller ranges
- for regression-based methods like linear regressionm we want targets to hvae a linear dependency on features
## [[Feature Cross]]
- synthetic feature formed by multiplying or crossing two or more features
## Numeric Features
- what happens if we train [[Ridge]] on a dataset without applying transformations to the categorical features?
	- error! **linear models require all features in numeric form**