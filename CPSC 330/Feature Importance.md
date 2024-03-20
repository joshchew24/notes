# Feature Importance
- how does the output depend on the input
	- i.e. how do the predictions change as a **function of a particular feature**
## Correlations
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