# Feature Importance
- how does the output depend on the input
	- i.e. how do the predictions change as a **function of a particular feature**
## Correlations
- **Correlation** among features might make coefficients completely **uninterpretable**. 
- Fairly **straightforward** to interpret coefficients of **ordinal** features. 
- In **categorical** features, it's often helpful to consider **one category as a reference** point and think about relative importance. 
- For **numeric** features, relative importance is meaningful **after scaling**.
	- You have to be careful about the scale of the feature when interpreting the coefficients. 
- Remember that explaining the model $\neq$ explaining the data or explaining how the world works.
	- The **coefficients** tell us only about the **model** and they might **not** accurately reflect the **data**. 
### Simple Interpretation
- if $Y$ goes up when $X$ goes up, they are **positively correlated**
- if $Y$ goes down when $X$ goes up, they are **negatively correlated**
- if $Y$ is unchanged when $X$ changes, they are **uncorrelated**
### Example
- Using [[Seaborn Heatmap]] to examine correlations between various features with other features and the target in our encoded data (first row/column)
```python
cor = pd.concat((y_train, X_train_enc), axis=1).iloc[:, :10].corr()
plt.figure(figsize=(8, 6))
sns.set(font_scale=0.8)
sns.heatmap(cor, annot=True, cmap=plt.cm.Blues);
```
![[Pasted image 20240320154216.png|450]]
- `SalePrice` is highly correlated with `OverallQual`.
	- early hint that `OverallQual` is a useful feature in predicting `SalePrice`.
- this approach is **extremely simplistic**.
	- It only looks at **each feature in isolation**.
	- It only looks at **linear associations**:
		- What if `SalePrice` is high when `BsmtFullBath` is 2 or 3, but low when it's 0, 1, or 4? They might seem uncorrelated.
```python
cor = pd.concat((y_train, X_train_enc), axis=1).iloc[:, 10:15].corr()
plt.figure(figsize=(4, 4))
sns.set(font_scale=0.8)
sns.heatmap(cor, annot=True, cmap=plt.cm.Blues);
```
### Feature Interaction
![[Pasted image 20240320155208.png]]
- Looking at this diagram also tells us the relationship between features. 
	- For example, `1stFlrSF` and `TotalBsmtSF` are highly correlated. 
	- Do we need both of them?
	- If our model says `1stFlrSF` is very important and `TotalBsmtSF` is very unimportant, do we trust those values?
	- Maybe `TotalBsmtSF` only "becomes important" if `1stFlrSF` is removed.
	- Sometimes the opposite happens: a feature only becomes important if another feature is _added_.
## Linear Models
- With linear regression we can look at the _coefficients_ for each feature.
	- Overall idea: predicted price = intercept + $\sum_i$ coefficient i $\times$ feature i.
### Interpreting Coefficients of Different Types of Features
```python
lr = make_pipeline(preprocessor, Ridge())
lr.fit(X_train, y_train)

lr_coefs = pd.DataFrame(
    data=lr.named_steps["ridge"].coef_, index=new_columns, columns=["Coefficient"]
)
lr_coefs.head(20)

lr_coefs.loc[["ExterQual"]]
```
#### Ordinal Features
- increasing by one category of an ordinal feature increases the prediction by the coefficient value
	- **according to the model's learning**
		- can verify by checking one example, and altering the value of a category to see how it affects the prediction
#### Categorical Features
- each category gets its own column and coefficient (because of [[One-hot encoding]])
- we can compare categories by picking a "reference" category
	- If you did `drop='first'` in one hot encoding (we didn't) then you already have a reference class, and all the values are with respect to that one. The interpretation depends on whether we did `drop='first'`, hence the hassle.
```python
lr_coefs_landslope = lr_coefs[lr_coefs.index.str.startswith("LandSlope")]
lr_coefs_landslope

lr_coefs_landslope.loc[["LandSlope_Gtl"]]
# 468.638169

lr_coefs_landslope - lr_coefs_landslope.loc["LandSlope_Gtl"]
```
![[Pasted image 20240320161502.png]]
- If you change the category from `LandSlope_Gtl` to `LandSlope_Mod` the prediction price goes up by $\sim\$6950$
- If you change the category from `LandSlope_Gtl` to `LandSlope_Sev` the prediction price goes down by $\sim\$8356$
- *Note that this might not make sense in the real world but this is what our model decided to learn given this small amount of data.*
#### Numerical Features
- can be tricky since numerical features are **scaled**
	- increasing by **1 scaled unit** affects the prediction by the coefficient
##### Example
- What's **one scaled unit** for `LotArea`? 
- The scaler **subtracted** the mean and **divided** by the standard deviation.
- The **division** actually **changed the scale**! 
- For the **unit conversion**, we don't care about the subtraction, but only the scaling (by division).
```python
scaler = preprocessor.named_transformers_["pipeline-1"]["standardscaler"]

scaler.var_

lr_scales = pd.DataFrame(
    data=np.sqrt(scaler.var_), index=numeric_features, columns=["Scale"]
)
lr_scales.head()

lr_coefs.loc[["LotArea"]]

X_test_enc = pd.DataFrame(
    preprocessor.transform(X_test), index=X_test.index, columns=new_columns
)

one_ex_preprocessed = X_test_enc[:1]
one_ex_preprocessed

orig_pred = lr.named_steps["ridge"].predict(one_ex_preprocessed.values)
orig_pred

one_ex_preprocessed_perturbed = one_ex_preprocessed.copy()
one_ex_preprocessed_perturbed["LotArea"] += 1  # we are adding one to the scaled LotArea
one_ex_preprocessed_perturbed

perturbed_pred = lr.named_steps["ridge"].predict(one_ex_preprocessed_perturbed.values)

perturbed_pred - orig_pred
```
- Humans find it easier to think about features in their original scale.  
- How can we interpret this coefficient in the original scale? 
- If I increase original `LotArea` by one square foot then the predicted price would go up by $0.57
```python
5118.035161 / 8994.471032 # Coefficient learned on the scaled features / the scaling factor for this feature
```


## `feature_importances_`
- **Feature importance** or **variable importance** is a score associated with a feature which tells us how "important" the feature is to the model.
- Feature importances can be
    - algorithm dependent, i.e., calculated based on the information given by the model algorithm (e.g., gini importance)
    - model agnostic (e.g., by measuring increase in prediction error after permuting feature values).
- Different measures give insight into different aspects of your data and model.
- Unlike the linear model coefficients, `feature_importances_` **do not have a sign**!
  - They tell us about **importance**, but ***not*** an "**up** or **down**".
  - Indeed, increasing a feature may cause the prediction to first go up, and then go down.
  - This cannot happen in linear models, because they are linear. 
> [Here](https://scikit-learn.org/stable/modules/permutation_importance.html#relation-to-impurity-based-importance-in-trees) you will find some drawbacks of using `feature_importances_` attribute in the context of tree-based models.
### `permutation_importance`
```python
from sklearn.inspection import permutation_importance
def get_permutation_importance(model):
    X_train_perm = X_train.drop(columns=["race", "education.num", "fnlwgt"])
    result = permutation_importance(model, X_train_perm, y_train_num, n_repeats=10, random_state=123)
    perm_sorted_idx = result.importances_mean.argsort()
    plt.boxplot(
        result.importances[perm_sorted_idx].T,
        vert=False,
        labels=X_train_perm.columns[perm_sorted_idx],
    )
    plt.xlabel('Permutation feature importance')
    plt.show()
```
![[Pasted image 20240320172124.png]]