# Survival Analysis
- interested in the **time until an event occurs**
	- examples
		- the time until a customer leaves a subscription service (see [[Customer Churn]])
		- the time until a disease kills its host
		- the time until a piece of equipment breaks
		- the time that someone unemployed will take to land a new job
		- the time until you wait for your turn to get a surgery
	- i.e. **whether or not something will happen in a certain time frame**
- use `lifelines` package
## Kaplan-Meier Survival Curve
- using base example from [[Customer Churn]]
- drop `TotalCharges`
	- value changes over time, but we only haev the final value
	- still have `MonthlyCharges`
- not scaling `tenure`, convenient to keep it in original unit of months
```python
numeric_features = ["MonthlyCharges"]
drop_features = ["customerID", "TotalCharges"]
passthrough_features = ["tenure", "SeniorCitizen"]  # don't want to scale tenure
target_column = ["Churn"]
# the rest are categorical
categorical_features = list(
    set(train_df.columns)
    - set(numeric_features)
    - set(passthrough_features)
    - set(drop_features)
    - set(target_column)
)

preprocessing_final = make_column_transformer(
    (
        FunctionTransformer(lambda x: x == "Yes"),
        target_column,
    ),  # because we need it in this format for lifelines package
    ("passthrough", passthrough_features),
    (StandardScaler(), numeric_features),
    (OneHotEncoder(handle_unknown="ignore", sparse_output=False), categorical_features),
    ("drop", drop_features),
)

preprocessing_final.fit(train_df)

new_columns = (
    target_column
    + passthrough_features
    + numeric_features
    + preprocessing_final.named_transformers_["onehotencoder"]
    .get_feature_names_out(categorical_features)
    .tolist()
)

train_df_surv = pd.DataFrame(
    preprocessing_final.transform(train_df), index=train_df.index, columns=new_columns
)
test_df_surv = pd.DataFrame(
    preprocessing_final.transform(test_df), index=test_df.index, columns=new_columns
)
```