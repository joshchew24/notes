# Random Forests
> [!information] General Idea
> - a single decision tree is likely to overfit
> - use a **collection of diverse** decision trees
> - each tree overfits on some part of the data 
> 	- reduce overfitting by averaging results
> 	- can be shown mathematically

## `RandomForestClassifier`
```python
from sklearn.ensemble import RandomForestClassifier

pipe_rf = make_pipeline(
    preprocessor,
    RandomForestClassifier(
        n_jobs=-1,
        random_state=123,
    ),
)
results["Random forests"] = mean_std_cross_val_scores(
    pipe_rf, X_train, y_train_num, return_train_score=True, scoring=scoring_metric
)
pd.DataFrame(results).T
```
- `n_estimators` hyperparameter controls how many decision trees to build
- `fit` a **diverse set** of that many decision trees by **injecting randomness** in the model construction
- `predict` by **voting** (classification) or **averaging** (regression) of predictions given by individual models
### Injecting Randomness
- two different ways
	- data
		- build each tree on a [[Bootstrap Samples|bootstrap sample]]
			- i.e. sample drawn **with replacement** from the training set
	- features
		- at each node, select a random subset of features
			- controlled by `max_features`
			- look for the **best possible test** involving one of these features