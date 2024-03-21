---
aliases:
  - SHAP
---
# SHapley Additive exPlanations (SHAP)
- Based on the idea of shapely values. A shapely value is created for each example and each feature. 
- Can explain the prediction of an example by computing the contribution of each feature to the prediction. 
- Great visualizations 
- Support for different kinds of models; fast variants for tree-based models
- You can think of this as **global feature importance**
- Original paper: [Lundberg and Lee, 2017](https://arxiv.org/pdf/1705.07874.pdf)
```python
import shap

# Create a shap explainer object 
pipe_lgbm.named_steps["lgbmclassifier"].fit(X_train_enc, y_train)
lgbm_explainer = shap.TreeExplainer(pipe_lgbm.named_steps["lgbmclassifier"])
train_lgbm_shap_values = lgbm_explainer.shap_values(X_train_enc)
```
- For each example, each feature, and each class we have a SHAP value.
	- SHAP values tell us how to fairly distribute the prediction among features. 
- For classification it's a bit confusing. It gives SHAP matrix for all classes.
## Example
sampling examples where target is <= 50k and some where target is >50k
```python
y_test_reset = y_test.reset_index(drop=True)
y_test_reset

l50k_ind = y_test_reset[y_test_reset == "<=50K"].index.tolist()
g50k_ind = y_test_reset[y_test_reset == ">50K"].index.tolist()

ex_l50k_index = l50k_ind[10]
ex_g50k_index = g50k_ind[10]
```
given the following test example, we get the hard prediction of <=50k, which we are interested in explaining.
```python
X_test_enc.iloc[ex_l50k_index]
pipe_lgbm.named_steps["lgbmclassifier"].predict(X_test_enc)[ex_l50k_index]
```
We can look at `predict_proba` to see model confidence
```python
pipe_lgbm.named_steps["lgbmclassifier"].predict_proba(X_test_enc)[ex_l50k_index]
```