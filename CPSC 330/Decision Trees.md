# Decision Trees
- [[Tree-based model]]

Learn a binary decision tree to predict the outcome
![[Pasted image 20240118223710.png]]
- note: this example uses discrete/binary features, so the decision threshold is always `0.5`
- decision trees can be created with continuous features, resulting in varying thresholds
## [[Feature Selection]]
- which features are most useful for classification?
- aim to minimize **impurity** at each node
	- lower impurity indicates a more **homogenous** or **pure** set of data points
	- i.e. we aim to create subsets that are more homogenous with respect to the target variable
	- By default, leaf nodes are **pure**
### Criteria to Minimize Impurity
- [[Gini Index]]
- information gain
- cross entropy

## [[Regression]]
- instead of [[Gini Index|gini]], we use other criteria for splitting
	- commonly use [[Mean Squared Error (MSE)]]
- [[sklearn]] supports regression using decision trees with `DecisionTreeRegressor`
## [[Parameters]]
- the best feature to split on
- the threshold for the feature to split on at each node
## [[Hyperparameters]]
### Max Depth
- a super deep decision node may be too closely fitted to the training data
	- i.e. a single unique training data point is serving as the only evidence for a prediction
#### Decision Stump
- a decision tree with only one split (`depth = 1`)
```python
model = DecisionTreeClassifier(max_depth=1)
model.fit(X, y)
display_tree(X.columns, model, counts=True)
```
![[Pasted image 20240118230816.png]]
### Others
- `min_samples_split`
- `min_samples_leaf`
- `max_leaf_nodes`