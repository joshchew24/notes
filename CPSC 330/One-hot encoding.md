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
# One-hot encoding
ohe.categories_
# see dataset with new features
transformed_ohe = pd.DataFrame(
    data=X_imp_ohe_train,
    columns=ohe.get_feature_names_out(["ocean_proximity"]),
    index=X_train.index,
)
transformed_ohe
```
## Handling Unknown Values
- it's possible to have features that only get put into the validation split and are not found in the training split
### Simplest fix
- Pass `handle_unknown="ignore"` argument to `OneHotEncoder`
- It creates a row with all zeros. 
- With this approach, **all unknown categories will be represented with all zeros** and cross-validation is running OK now. 
```python
ct = make_column_transformer(
    (
        make_pipeline(SimpleImputer(), StandardScaler()),
        numeric_feats,
    ),  # scaling on numeric features
    (
        OrdinalEncoder(categories=[class_attendance_levels], dtype=int),
        ordinal_feats,
    ),  # Ordinal encoding on ordinal features
    ("passthrough", passthrough_feats),  # no transformations on the binary features
    (
        OneHotEncoder(handle_unknown="ignore"),
        categorical_feats,
    ),  # OHE on categorical features    
    ("drop", drop_feats),  # drop the drop features
)
```
### Breaking the Golden Rule
If we **know categories beforehand** we can specify them to `OneHotEncoder` to avoid errors during `cross_over` assessments. However, wouldn't that be ***breaking the golden*** rule?

When we know the categories in advance and this is **one of the cases where it might be OK to violate the golden rule** and get a list of all possible values for the categorical variable. 

For example, if it's some fix number of categories. E.g., if it's something like:
  -  provinces in Canada or 
  -  majors taught at UBC. 
## Binary Categories
- if a feature is binary, we can pass `drop="if_binary"` to `OneHotEncoder`
	- creates a single binary column to represent the feature
```python
ohe_enc = OneHotEncoder(drop="if_binary", dtype=int, sparse_output=False)
ohe_enc.fit(X[["enjoy_course"]])
transformed = ohe_enc.transform(X[["enjoy_course"]])
df = pd.DataFrame(data=transformed, columns=["enjoy_course_enc"], index=X.index)
pd.concat([X[["enjoy_course"]], df], axis=1).head(10)`
```

With a [[Column Transformer]]:
```python
numeric_feats = [
    "university_years",
    "lab1",
    "lab2",
    "lab3",
    "lab4",
    "quiz1",
]  # apply scaling
categorical_feats = ["major"]  # apply one-hot encoding
ordinal_feats = ["class_attendance"]  # apply ordinal encoding
binary_feats = ["enjoy_course"]  # apply one-hot encoding with drop="if_binary"
passthrough_feats = ["ml_experience"]  # do not apply any transformation
drop_feats = []

ct = make_column_transformer(
    (
        make_pipeline(SimpleImputer(), StandardScaler()),
        numeric_feats,
    ),  # scaling on numeric features
    (
        OrdinalEncoder(categories=[class_attendance_levels], dtype=int),
        ordinal_feats,
    ),  # Ordinal encoding on ordinal features
    (
        OneHotEncoder(drop="if_binary", dtype=int),
        binary_feats,
    ),  # OHE on categorical features
    ("passthrough", passthrough_feats),  # no transformations on the binary features    
    (
        OneHotEncoder(handle_unknown="ignore"),
        categorical_feats,
    ),  # OHE on categorical features
)
```
## Many categories
- Do we have enough data for rare categories to learn anything meaningful? 
- How about **grouping them into bigger categories**?
    - Example: country names into continents such as "South America" or "Asia"
- Or having **"other" category for rare cases**? 
## Do we actually want to use certain features for prediction?
- Do you **want** to use certain features such as **gender** or **race** in prediction?
- Remember that the systems you build are going to be used in some applications. 
- It's extremely important to be mindful of the consequences of including certain features in your predictive model. 
## Preprocessing the targets?
- Generally no need for this when doing classification. 
- In regression it makes sense in some cases. More on this later. 
- `sklearn` is fine with categorical labels ($y$-values) for classification problems. 
## Sparse Features
- By default, `OneHotEncoder` also creates sparse features. 
- You could set `sparse=False` to get a regular `numpy` array. 
- If there are a huge number of categories, it may be beneficial to keep them sparse.
- For smaller number of categories, it doesn't matter much.