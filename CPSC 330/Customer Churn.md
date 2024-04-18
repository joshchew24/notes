---
aliases:
  - Customer Attrition
---
# Customer Churn
- refers to the phenomenon where customers or subscribers stop doing business with a company or service
## Example Binary Classification
We naively try binary classification:

[Customer Churn Dataset](https://www.kaggle.com/blastchar/telco-customer-churn)
 ```python
 df = pd.read_csv("../data/WA_Fn-UseC_-Telco-Customer-Churn.csv")
train_df, test_df = train_test_split(df, random_state=123)
train_df.head()
train_df.info()
```
Some values in `TotalCharges` column are whitespace:
```python
for val in train_df["TotalCharges"]:
    try:
        float(val)
    except ValueError:
        print('"%s"' % val)
```
Replace with NaN:
```python
train_df = train_df.assign(
    TotalCharges=train_df["TotalCharges"].replace(" ", np.nan).astype(float)
)
test_df = test_df.assign(
    TotalCharges=test_df["TotalCharges"].replace(" ", np.nan).astype(float)
)
train_df.info()
```
Preprocess data, making sure to impute newly missing values
```python
preprocessor = make_column_transformer(
    (
        make_pipeline(SimpleImputer(strategy="median"), StandardScaler()),
        numeric_features,
    ),
    (OneHotEncoder(handle_unknown="ignore"), categorical_features),
    ("passthrough", passthrough_features),
    ("drop", drop_features),
)
preprocessor.fit(train_df)
new_columns = (
    numeric_features
    + preprocessor.named_transformers_["onehotencoder"]
    .get_feature_names_out(categorical_features)
    .tolist()
    + passthrough_features
)
X_train_enc = pd.DataFrame(
    preprocessor.transform(train_df), index=train_df.index, columns=new_columns
)
X_test_enc = pd.DataFrame(
    preprocessor.transform(train_df), index=train_df.index, columns=new_columns
)

X_train = train_df.drop(columns=["Churn"])
X_test = test_df.drop(columns=["Churn"])

y_train = train_df["Churn"]
y_test = test_df["Churn"]
```
## Problem
- when treating this as binary classification, we predict whether the customer would churn or not at a specific point in time (when the data was collected)
- it's more useful to understand **when** they are likely to churn
	- to offer promotions, etc
	- see [[Survival Analysis]]
- can't do regular analysis of `tenure` column in combination with `churn` column
	- ![[Pasted image 20240417190531.png|172]]
	- we don't know the actual time to churn in all occurrences
		- i.e. **Censoring** prevents us from observing the exact time the event happened for all examples
		- we don't have the **correct target values** to train and test our model
## Approaches
### Approach 1
- only consider examples where `churn=yes
- 