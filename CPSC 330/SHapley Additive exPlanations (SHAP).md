---
aliases:
  - SHAP
---
# SHapley Additive exPlanations (SHAP)
- Based on the idea of shapely values. A shapely value is created for each example and each feature. 
- Can explain the prediction of an example by computing the contribution of each feature to the prediction. 
- Great visualizations 
- Support for different kinds of models; fast variants for tree-based models
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
- Let's stick to shap values for class 1, i.e., income > 50K. 