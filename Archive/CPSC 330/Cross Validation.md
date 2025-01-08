# Cross Validation
- split the data into $k$ folds ($k > 2$, often $k = 10$)
- independently validate the model using each split
- analyze the mean and stddev of the different split scores

## `cross_val_score`
1. Imports
```python
from sklearn.model_selection import cross_val_score, cross_validate
```
2. Create the model and cross validate it
```python
model = DecisionTreeClassifier(max_depth=4)
cv_scores = cross_val_score(model, X_train, y_train, cv=10)
```
3. Find the mean and stddev of the CV scores
```python
cv_score_mean = np.mean(cv_scores)
cv_score_err = np.std(cv_scores)
```
## `cross_validate`
- similar to `cross_val_score` but more powerful
- gives access to training and validation scores

Example:
```python
scores = cross_validate(model, X_train, y_train, cv=10, return_train_score=True)
pd.DataFrame(scores)
```
Output:

| (idx) | `fit_time` | `score_time` | `test_score` | `train_score` |
| ---- | ---- | ---- | ---- | ---- |
| 0 | time to fit | time to score | validation score | training score |
Mean:
```python
pd.DataFrame(pd.DataFrame(scores).mean())
```