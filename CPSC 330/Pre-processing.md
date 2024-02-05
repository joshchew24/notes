# Pre-processing
- need to pre-process data because features can have different scales that heavily bias a model
	- i.e. features with large values dominate features with smaller values
- we use `transformer`s to transform our data
## [`StandardScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)
```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()  # create feature trasformer object
scaler.fit(X_train)  # fitting the transformer on the train split
X_train_scaled = scaler.transform(X_train)  # transforming the train split
X_test_scaled = scaler.transform(X_test)  # transforming the test split
```
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
### Scaling
- scaling of numeric features
### One-hot encoding
- tackling categorical variables