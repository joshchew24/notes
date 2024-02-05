# Pre-processing
- need to pre-process data because features can have different scales that heavily bias a model
	- i.e. features with large values dominate features with smaller values
- we use `transformer`s to transform our data

## `fit` and `transform` paradigm for transformers
- `sklearn` uses `fit` and `transform` paradigms for feature transformations
- We `fit` the transformer on the train split and then transform the train split as well as the test split
- We apply the same transformations on the test split
- You can pass `y_train` in `fit` but it's usually ignored. It allows you to pass it just to be consistent with usual usage of `sklearn`'s `fit` method.   
- You can also carry out fitting and transforming in one call using `fit_transform`. But be mindful to use it **only** on the **train split** and **not** on the **test split**.
## Common Techniques
### Imputation
- tackling missing values
- replace with reasonable value
	- `SimpleImputer`
		- impute missing values in categorical columns with the most frequent value
		- impute missing values in numeric columns with the mean or median of the column
#### [`SimpleImputer`](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html)
```python
imputer = SimpleImputer(strategy="median")
imputer.fit(X_train)
X_train_imp = imputer.transform(X_train)
X_test_imp = imputer.transform(X_test)
```

### Scaling
- scaling of numeric features
- this problem affects a large number of ML methods
- many approaches, most popular: normalization and standardization

| Approach | What it does | How to update $X$ (but see below!) | sklearn implementation | 
|---------|------------|-----------------------|----------------|
| standardization | sets sample mean to $0$, s.d. to $1$   | `X -= np.mean(X,axis=0)`<br>`X /=  np.std(X,axis=0)` | [`StandardScaler()`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler) |
#### [`StandardScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)
```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()  # create feature trasformer object
scaler.fit(X_train)  # fitting the transformer on the train split
X_train_scaled = scaler.transform(X_train)  # transforming the train split
X_test_scaled = scaler.transform(X_test)  # transforming the test split
```
### [[examine new features]]
## [[Cross Validation]]
```python
knn = KNeighborsRegressor()

scaler = StandardScaler()
scaler.fit(X_train_imp)
X_train_scaled = scaler.transform(X_train_imp)
X_test_scaled = scaler.transform(X_test_imp)
scores = cross_validate(knn, X_train_scaled, y_train, return_train_score=True)
pd.DataFrame(scores)
```
> [!danger] Danger
> by passing `X_train_scaled` to the `cross_validator`, we are creating validation splits from the fitted/transformed data, which violates the golden rule
- use [[Pipelines]]
## [[Column Transformer]]