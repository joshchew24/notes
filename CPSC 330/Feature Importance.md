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

