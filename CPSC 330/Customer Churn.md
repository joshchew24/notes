---
aliases:
  - Customer Attrition
---
# Customer Churn
- refers to the phenomenon where customers or subscribers stop doing business with a company or service
## Example Binary Classification
[Customer Churn Dataset](https://www.kaggle.com/blastchar/telco-customer-churn)
 ```python
 df = pd.read_csv("../data/WA_Fn-UseC_-Telco-Customer-Churn.csv")
train_df, test_df = train_test_split(df, random_state=123)
train_df.head()

