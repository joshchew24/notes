# Feature Engineering
> [!Definition]
> The process of **transforming raw data into features** that better represent the underlying problem to the predictive models, resulting in improved model accuracy on unseen data

## Garbage In, Garbage Out
- Better features usually help more than a better model
- good features:
	- capture **most important aspects** of the problem
	- allow learning with **few examples**
	- **generalize** to new scenarios
- **trade-off** between **simple** and **expressive** features
	- **simple features** have lower scores with less risk of overfitting
	- **complicated features** have higher scores with more risk of overfitting
## Model Dependent
- the best features may depend on what model you use
- for counting-based methods like decision trees, seperate relevant **groups of variable values**
	- discretization
		- partitioning and converting continuous attributes into discrete intervals
		- enables the use of continuous features in algorithms that require discrete features
- for distance-based methods like kNN, we want different class labels to be "far"
	- standardization
		- avoid dominance of wide-ranging features over other features with smaller ranges
- for regression-based methods like linear regressionm we want targets to hvae a linear dependency on features
## [[Feature Cross]]
- synthetic feature formed by multiplying or crossing two or more features
## Numeric Features
- what happens if we train [[Ridge]] on a dataset without applying transformations to the categorical features?
	- error! **linear models require all features in numeric form**
### Discretization
- bucketing or binning
	- transforming numeric features into categorical features
- `KBinsDiscretizer`
```python
from sklearn.preprocessing import KBinsDiscretizer

discretization_feats = ["latitude", "longitude"]
numeric_feats = ["rooms_per_household"]

preprocessor2 = make_column_transformer(
    (KBinsDiscretizer(n_bins=20, encode="onehot"), discretization_feats),
    (make_pipeline(SimpleImputer(), StandardScaler()), numeric_feats),
)

lr_2 = make_pipeline(preprocessor2, Ridge())
pd.DataFrame(
    cross_validate(lr_2, X_train_housing, y_train_housing, return_train_score=True)
)

lr_2.fit(X_train_housing, y_train_housing)

pd.DataFrame(
    preprocessor2.fit_transform(X_train_housing).todense(),
    columns=preprocessor2.get_feature_names_out(),
)
```
Discretizing all three features:
```python
from sklearn.preprocessing import KBinsDiscretizer

discretization_feats = ["latitude", "longitude", "rooms_per_household"]

preprocessor3 = make_column_transformer(
    (KBinsDiscretizer(n_bins=20, encode="onehot"), discretization_feats),
)

lr_3 = make_pipeline(preprocessor3, Ridge())
pd.DataFrame(
    cross_validate(lr_3, X_train_housing, y_train_housing, return_train_score=True)
)

lr_3.fit(X_train_housing, y_train_housing)