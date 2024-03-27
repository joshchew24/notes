# Ridge
- similar to [[Linear Regression]]
	- prediction formula is same one used for "ordinary least squares"
- "vanilla" linear regression may result in large coefficients and unexpected results. 
- So instead of using `LinearRegression`, we will _always use another linear model called `Ridge`_, which is a linear regression model with a complexity **hyperparameter** `alpha`.
- this hyperparameter is what makes `Ridge` different from `LinearRegression`
	- `alpha=0` is the same as using `LinearRegression`
```python
from sklearn.linear_model import LinearRegression  # DO NOT USE IT IN THIS COURSE
from sklearn.linear_model import Ridge  # USE THIS INSTEAD
```
## Example
```python
from sklearn.datasets import fetch_california_housing


california = fetch_california_housing()
X_train, X_test, y_train, y_test = train_test_split(
    california.data, california.target, test_size=0.2
)
pd.DataFrame(X_train, columns=california.feature_names)

print(california.DESCR)

pipe = make_pipeline(StandardScaler(), Ridge())
scores = cross_validate(pipe, X_train, y_train, return_train_score=True)
pd.DataFrame(scores)

pd.DataFrame(scores).mean().rename('mean').to_frame().T
```
## `alpha` Hyperparameter
- inverse of `C` in [[Logistic Regression]]
- smaller `alpha`: lower training error, overfitting
- larger `alpha` leads to **smaller coefficients**
	- the predictions are **less sensitive to changes in the data**
	- i.e. **less chance of overfitting**
	- chance of underfitting
- controls the [[Bias and Variance Tradeoff|Fundamental Tradeoff]]

Examining the effect of `alpha` on the [[Bias and Variance Tradeoff|Fundamental Tradeoff]]
```python
scores_dict = {
    "alpha": 10.0 ** np.arange(-3, 6, 1),
    "mean_train_scores": list(),
    "mean_cv_scores": list(),
}
for alpha in scores_dict["alpha"]:
    pipe_ridge = make_pipeline(StandardScaler(), Ridge(alpha=alpha))
    scores = cross_validate(pipe_ridge, X_train, y_train, return_train_score=True)
    scores_dict["mean_train_scores"].append(scores["train_score"].mean())
    scores_dict["mean_cv_scores"].append(scores["test_score"].mean())

results_df = pd.DataFrame(scores_dict)
results_df.set_index('alpha')

# Plot only the top part of the table for better viewing
results_df.set_index('alpha').head(7).plot(logx=True);
```
![[Pasted image 20240212003917.png|317]]![[Pasted image 20240212003930.png|375]]

## Coefficients
- **one coefficient per feature** which describes the **role/weight** of the feature in the prediction according to the model
- **positive coefficient** means the prediction is ***proportional*** to the feature value
- **negative coefficient** means the prediction is ***inversely proportional*** to the feature value 
- **bigger magnitude** means the feature value is more impactful on the prediction
> [!danger] Scaling
> If you do not scale the data, features with smaller magnitude are going to get coefficients with bigger magnitude whereas features with bigger scale are going to get coefficients with smaller magnitude.
## Intercept
- we add this amount irrespective ot the feature values
## `RidgeCV`
- very common to tune `alpha` with cross-validation, `RidgeCV` automatically does this for us
### Example
```python
alphas = 10.0 ** np.arange(-6, 6, 1)
ridgecv_pipe = make_pipeline(preprocessor, RidgeCV(alphas=alphas, cv=10))
ridgecv_pipe.fit(X_train, y_train);

best_alpha = ridgecv_pipe.named_steps["ridgecv"].alpha_

ridge_tuned = make_pipeline(preprocessor, Ridge(alpha=best_alpha))
ridge_tuned.fit(X_train, y_train)
ridge_preds = ridge_tuned.predict(X_test)
ridge_preds[:10]

# get the features names of the transformed data
df = pd.DataFrame(
    data={"coefficients": ridge_tuned.named_steps["ridge"].coef_}, index=new_columns
)
df.sort_values("coefficients", ascending=False)
```