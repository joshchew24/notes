# Model-Based Selection
- use a supervised machine learning model to **judge the importance** for each feature
- keep only the most important ones
- supervised machine learning **model used for feature selection** can be **different** than the one used as the **final estimator**
- use a model that can calculate feature importances
- `SelectFromModel` transformer
	- selects features which have feature importances **greater than the provided threshold**
## Example using `RandomForestClassifier` 
- feature selection with threshold "median" of feature importances
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
