# ColumnTransformer
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
```
