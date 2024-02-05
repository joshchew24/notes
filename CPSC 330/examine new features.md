transforming categorical features to numerical ones
## [[Ordinal encoding]]
- occasionally recommended
- assign an integer to each unique categorical label
- useful when categorical labels exist on some scale
- e.g. `[cold, warm, hot]`. 
## One-hot encoding
- Create new binary columns to represent our categories.
- If we have $c$ categories in our column.
    - We create $c$ new binary columns to represent those categories.
- OHE variables AKA **dummy variables**
	- `pandas.get_dummies`
### `OneHotEncoder`
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