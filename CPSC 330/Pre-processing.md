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
### Encoding
- transforming categorical features to numerical ones
#### Ordinal encoding
- occasionally recommended
- assign an integer to each unique categorical label
##### Drawbacks
- imposes ordinality on categorical data
- For example, imagine when you are calculating distances. Is it fair to say that French and Hindi are closer than French and Spanish? 
- In general, label encoding is useful if there is ordinality in your data and capturing it is important for your problem, e.g., `[cold, warm, hot]`. 
#### One-hot encoding
- Create new binary columns to represent our categories.
- If we have $c$ categories in our column.
    - We create $c$ new binary columns to represent those categories.
- OHE variables AKA **dummy variables**
	- `pandas.get_dummies`
##### `OneHotEncoder`
```python
from sklearn.preprocessing import OneHotEncoder

enc = OneHotEncoder(handle_unknown="ignore", sparse_output=False)
enc.fit(X_toy)
X_toy_ohe = enc.transform(X_toy)
pd.DataFrame(
    data=X_toy_ohe,
    columns=enc.get_feature_names_out(["language"]),
    index=X_toy.index,
)
# examine new features
ohe.categories_
# see dataset with new features
transformed_ohe = pd.DataFrame(
    data=X_imp_ohe_train,
    columns=ohe.get_feature_names_out(["ocean_proximity"]),
    index=X_train.index,
)
transformed_ohe
```
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



