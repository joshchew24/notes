# Model-Based Selection
- use a supervised machine learning model to **judge the importance** for each feature
- keep only the most important ones
- supervised machine learning **model used for feature selection** can be **different** than the one used as the **final estimator**
- use a model that can calculate feature importances
## `SelectFromModel` Transformer
- selects features which have feature importances **greater than the provided threshold**
### Examples
- feature selection with threshold "median" of feature importances

using `RandomForestClassifier` 
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.feature_selection import SelectFromModel

select_rf = SelectFromModel(
    RandomForestClassifier(n_estimators=100, random_state=42), 
    threshold="median"
)
```
Can we use kNN to select features? NO, because it does not report feature importances.
```python
from sklearn.neighbors import KNeighborsClassifier
select_knn = SelectFromModel(
    KNeighborsClassifier(), 
    threshold="median"
)

pipe_lr_model_based = make_pipeline(
    StandardScaler(), select_knn, LogisticRegression(max_iter=1000)
)

#pd.DataFrame(
#    cross_validate(pipe_lr_model_based, X_train, y_train, return_train_score=True)#
#).mean()
```
Can we use SVC? Yes, with linear kernel, not with RBF.
```python
select_svc = SelectFromModel(
    SVC(), threshold="median"
)

# pipe_lr_model_based = make_pipeline(
#     StandardScaler(), select_svc, LogisticRegression(max_iter=1000)
# )

# pd.DataFrame(
#    cross_validate(pipe_lr_model_based, X_train, y_train, return_train_score=True)
# ).mean()
```
## Recursive Feature Elimination (RFE)
- Build a series of models
- At each iteration, discard the least important feature according to the model. 
- Computationally expensive
- Basic idea
    - fit model
    - find least important feature
    - remove
    - iterate
### Algorithm
1. Decide $k$, the number of features to select. 
2. Assign importances to features, e.g. by fitting a model and looking at `coef_` or `feature_importances_`.
3. Remove the least important feature.
4. Repeat steps 2-3 until only $k$ features are remaining.

Note that this is **not** the same as just removing all the less important features in one shot!

### Example
```python
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)

from sklearn.feature_selection import RFE

# create ranking of features
rfe = RFE(LogisticRegression(), n_features_to_select=5)
rfe.fit(X_train_scaled, y_train)
rfe.ranking_

print(rfe.support_)

print("selected features: ", cancer.feature_names[rfe.support_])
```
### `n_features_to_select`
How do we how many `n_features_to_select`? Use cross-validation: `RFECV`
```python
from sklearn.feature_selection import RFECV

rfe_cv = RFECV(LogisticRegression(max_iter=2000), cv=10)
rfe_cv.fit(X_train_scaled, y_train)
print(rfe_cv.support_)
print(cancer.feature_names[rfe_cv.support_])
```
- slow because there's cross-validation within cross-validation ($k^2\times n^2$)
## Other Methods
- Search and Score
- Forward or backward selection (wrapper methods)
- stochastic local search
	- inject randomness to explore new parts of search space
	- simulated annealing
	- genetic algorithms
## Warnings
- feature's relevance is only defined in the context of other features
	- adding/removing features can make features relevant/irrelevant
- if features can be predicted from other features, you cannot know which one to pick
	- you don't the direction of the dependency
- relevance of features does not have a causal relationship
- overconfidence
	- methods do not discover ground truth, just what features are helpful in your model