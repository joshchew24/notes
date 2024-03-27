# Model-Based Selection
- use a supervised machine learning model to **judge the importance** for each feature
- keep only the most important ones
- supervised machine learning **model used for feature selection** can be **different** than the one used as the **final estimator**
- use a model that can calculate feature importances
- `SelectFromModel` transformer
	- selects features which have feature importances **greater than 