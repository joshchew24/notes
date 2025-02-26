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
The model is very confident. we can know which feature values are playing a role in this specific prediction using SHAP force plots
```python
pd.DataFrame(
    test_lgbm_shap_values[1][ex_l50k_index, :],
    index=feature_names,
    columns=["SHAP values"],
)
shap.force_plot(
    lgbm_explainer.expected_value[1], # expected value for class 1. 
    test_lgbm_shap_values[1][ex_l50k_index, :], # SHAP values associated with the example we want to explain
    X_test_enc.iloc[ex_l50k_index, :], # Feature vector of the example 
    matplotlib=True,
)
```
![[Pasted image 20240320174407.png|700]]
- The raw model score is much smaller than the base value, which is reflected in the prediction of <= 50k class. 
	- raw model score is the output of a predictive model before it has been transformed to a probability or predicted value
	- base value is a reference value used for comparing the raw model score
- sex = 1.0, scaled age = 0.48 are pushing the prediction towards higher score. 
- education = 8.0, occupation_Other-service = 1.0 and marital.status_Married-civ-spouse = 0.0 are pushing the prediction towards lower score. 
## Global Feature Importance
Examine average SHAP values associated with each feature:
```python
values = np.abs(train_lgbm_shap_values[1]).mean(
    0
)  # mean of shapely values in each column
pd.DataFrame(data=values, index=feature_names, columns=["SHAP"]).sort_values(
    by="SHAP", ascending=False
)[:10]

shap.dependence_plot("age", train_lgbm_shap_values[1], X_train_enc)
```
![[Pasted image 20240320180709.png]]The The plot above shows effect of `age` feature on the prediction. 
- Each dot is a single prediction for examples above.
- The x-axis represents values of the feature age (scaled).
- The y-axis is the SHAP value for that feature, which represents how much knowing that feature's value changes the output of the model for that example's prediction. 
- Lower values of age have smaller SHAP values for class ">50K".
- Similarly, higher values of age also have a bit smaller SHAP values for class ">50K", which makes sense.  
- There is some optimal value of age between scaled age of 1 which gives highest SHAP values for for class ">50K". 
- Ignore the colour for now. The color corresponds to a second feature (education feature in this case) that may have an interaction effect with the feature we are plotting. 
```python
shap.summary_plot(train_lgbm_shap_values[1], X_train_enc)
```
![[Pasted image 20240320180828.png]]
The plot shows the most important features for predicting the class. It also shows the direction of how it's going to drive the prediction.  
- Presence of the marital status of Married-civ-spouse seems to have bigger SHAP values for class 1 and absence seems to have smaller SHAP values for class 1. 
- Higher levels of education seem to have bigger SHAP values for class 1 whereas smaller levels of education have smaller SHAP values. 
```python
shap.summary_plot(train_lgbm_shap_values[1], X_train_enc, plot_type="bar")
```
![[Pasted image 20240320181215.png]]
