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
## Injecting Randomness
- two different ways
	- data
		- build each tree on a [[Bootstrap Samples|bootstrap sample]]
			- i.e. sample drawn **with replacement** from the training set
	- features
		- at each node, select a random subset of features
			- controlled by `max_features`
			- look for the **best possible test** involving one of these features


## Process
### Training
- create a collection ([[Ensembles]]) of trees
	- grow each tree on an independent [[Bootstrap Samples|bootstrap sample]] from the data
- At each node:
    - Randomly select a **subset of features** out of all features (independently for each node).
    - Find the **best split** on the selected features.
    - Grow the trees to **maximum depth**.
### Prediction
- **vote** the trees to get predictions for new example

## Example
- Let's create a random forest with 3 estimators. 
- I'm using `max_depth=2` for easy visualization. 
```python
pipe_rf_demo = make_pipeline(
    preprocessor, RandomForestClassifier(max_depth=2, n_estimators=3, random_state=123)
)
pipe_rf_demo.fit(X_train, y_train_num);

# get the features names of transformed features
feature_names = (
    numeric_features
    + ordinal_features
    + binary_features
    + pipe_rf_demo.named_steps["columntransformer"]
    .named_transformers_["pipeline-2"]
    .named_steps["onehotencoder"]
    .get_feature_names_out(categorical_features)
    .tolist()
)
pd.DataFrame(columns=feature_names)  # Take a look at the feature names

# sample a test example where income > 50k
probs = pipe_rf_demo.predict_proba(X_test)
np.where(probs[:, 1] > 0.55)

test_example = X_test.iloc[[582]]
pipe_rf_demo.predict_proba(test_example)
print("Classes: ", pipe_rf_demo.classes_)
print("Prediction by random forest: ", pipe_rf_demo.predict(test_example))
transformed_example = preprocessor.transform(test_example)
pd.DataFrame(data=transformed_example.flatten(), index=feature_names)

# look at different trees created by random forest
# note that each tree looks at different set of features and slightly different data
for i, tree in enumerate(
    pipe_rf_demo.named_steps["randomforestclassifier"].estimators_
):
    print("\n\nTree", i + 1)
    display(custom_plot_tree(tree, feature_names, fontsize=12))
    print("prediction", tree.predict(preprocessor.transform(test_example)))
```
## Important Hyperparameters
- `n_estimators`: number of decision trees (higher = more complexity)
- `max_depth`: max depth of each decision tree (higher = more complexity)
- `max_features`: the number of features you get to look at each split (higher = more complexity)
