# Column Transformer
## `ColumnTransformer`
Used when we want to apply different transformations on different columns
- `make_column_transformer`
	- The syntax automatically names each step based on its class. 
```python
from sklearn.compose import ColumnTransformer, make_column_transformer

df = pd.read_csv("../data/quiz2-grade-toy-col-transformer.csv")

X = df.drop(columns=["quiz2"])
y = df["quiz2"]

# identify the transformations we want to apply
X.head()
numeric_feats = ["university_years", "lab1", "lab3", "lab4", "quiz1"]  # apply scaling
categorical_feats = ["major"]  # apply one-hot encoding
passthrough_feats = ["ml_experience"]  # do not apply any transformation
drop_feats = [
    "lab2",
    "class_attendance",
    "enjoy_course",
]  # do not include these features in modeling

# create a column transfrormer
ct = ColumnTransformer(
    [
        ("scaling", StandardScaler(), numeric_feats),
        ("onehot", OneHotEncoder(sparse_output=False), categorical_feats),
    ]
)

# use convenient syntax
ct = make_column_transformer(    
    (StandardScaler(), numeric_feats),  # scaling on numeric features
    ("passthrough", passthrough_feats),  # no transformations on the binary features    
    (OneHotEncoder(), categorical_feats),  # OHE on categorical features
    ("drop", drop_feats),  # drop the drop features
)

transformed = ct.fit_transform(X)
```
- When we `fit_transform`, each transformer is applied to the specified columns and the result of the transformations are concatenated horizontally. 
- A big advantage here is that we build all our transformations together into one object, and that way we're sure we do the same operations to all splits of the data.
- Otherwise we might, for example, do the OHE on both train and test but forget to scale the test data.
## Convert to Dataframe
Since we are adding more columns, the original columns don't map directly to the transformed data.
```python
ct.named_transformers_
# E.g., columns preprocessed by StandardScaler
ct.named_transformers_["standardscaler"].get_feature_names_out()
# Here are the new columns created by OneHotEncoder
ct.named_transformers_["onehotencoder"].get_feature_names_out()
column_names = (
    numeric_feats
    + passthrough_feats    
    + ct.named_transformers_["onehotencoder"].get_feature_names_out().tolist()
)

pd.DataFrame(transformed, columns=column_names).head()
```
Note that the order of the columns in the transformed data depends upon the order of the features we pass to the `ColumnTransformer` and can be different than the order of the features in the original dataframe.  

![[Pasted image 20240205035048.png]]
## Pipeline
We can use the column transformer in a pipeline:
```python
pipe = make_pipeline(ct, SVC())
pipe.fit(X, y)
pipe.predict(X)
```

To apply **more than one transformations** we can **define a pipeline inside a column transformer** to **chain different transformations**.
```python
ct = make_column_transformer(
    (
        make_pipeline(SimpleImputer(), StandardScaler()),
        numeric_feats,
    ),  # impuite and scale on numeric features
    ("passthrough", passthrough_feats),  # no transformations on the binary features    
    (OneHotEncoder(), categorical_feats),  # OHE on categorical features
    ("drop", drop_feats),  # drop the drop features
)
```
## `set_config`
- With multiple transformations in a column transformer, it can get tricky to keep track of everything happening inside it.  
- We can use `set_config` to display a diagram of this. 
```python
from sklearn import set_config
set_config(display="diagram")
ct
```
