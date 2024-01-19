# sklearn
## Classifiers
1. Read the data
```python
example_df = pd.read_csv('data/example_data.csv')
```
2. Create $X$ and $y$
```python
X = example_df.drop(columns="target_column")
y = example_df["target_column"]
```
3. Create a classifier object
```python
from sklearn.dummy import DummyClassifier             # import the classifier

dummy_clf = DummyClassifier(strategy="most_frequent")  # create the classifier
```
4. `fit` the classifier
```python
dummy_clf.fit(X, y)  # fit the classifier
```
5. `predict` on new examples
```python
dummy_clf.predict(X) # predict using the learner classifier
```
6. `score` the model
	- how can we evaluate the efficacy of our model?
	- for classification problems, by default, `score` gives the **accuracy** of the model
		- i.e. proportion of correctly predicted targets
		- $accuracy=\frac{correct\;predictions}{total\;examples}$ 
		- **error** is $1 - accuracy$
	- process
		- calls `predict` on $X$
		- compares predictions with $y$ (true targets)
		- returns the accuracy in case of classification
```python
dummy_clf.score(X, y)
```
## Regressors
- `fit` and `predict` are similar to classification
- `score` returns $R^2$ score
	- measure of closeness between true value and prediction
	- maximum $R^2$ is 1 for perfect predictions
	- can be negative which is very bad (worse than `DummyRegressor`)
- `DummyRegressor` returns the mean of the $y$ values