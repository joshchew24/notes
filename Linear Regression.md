# Linear Regression
## Example
Predicting weight of a snake given its length
```python
np.random.seed(7)
n = 100
X_1 = np.linspace(0, 2, n) + np.random.randn(n) * 0.01
X = pd.DataFrame(X_1[:, None], columns=["length"])

y = abs(np.random.randn(n, 1)) * 3 + X_1[:, None] * 5 + 0.2
y = pd.DataFrame(y, columns=["weight"])
snakes_df = pd.concat([X, y], axis=1)
train_df, test_df = train_test_split(snakes_df, test_size=0.2, random_state=77)

X_train = train_df[["length"]].values
y_train = train_df["weight"].values
X_test = test_df[["length"]].values
y_test = test_df["weight"].values
train_df.head()

plt.plot(X_train, y_train, ".", markersize=10)
plt.xlabel("length")
plt.ylabel("weight (target)");

grid = np.linspace(min(X_train)[0], max(X_train)[0], 1000)
grid = grid.reshape(-1, 1)

from sklearn.linear_model import Ridge

r = Ridge()
r.fit(X_train, y_train)
plt.plot(X_train, y_train, ".", markersize=10)
plt.plot(grid, r.predict(grid))
plt.grid(True)
plt.xlabel("length")
plt.ylabel("weight (target)");
```
![[Pasted image 20240212001057.png]]

Orange line is the learned linear model.
## Model
- learning a line
	- represented by a **slope** coefficient/weight and an **intercept**
	- `r.coef_` and `r.intercept_`

> [!important] Given a feature value $x_1$ and learned coefficient $w_1$ and intercept $b$, we can get the prediction $\hat{y}$ with the following formula:
$$\hat{y} = w_1x_1 + b$$
> i.e.
> ` prediction = snake_length * r.coef_ + r.intercept_ `
> 
### Generalizing to more features
> [!important] For more features, the model is a higher dimensional hyperplane and the general prediction formula looks as follows: 
$\hat{y} =$ <font color="red">$w_1$</font> <font color="DodgerBlue">$x_1$ </font> $+ \dots +$ <font color="red">$w_d$</font> <font color="DodgerBlue">$x_d$</font> + <font  color="green"> $b$</font>
>
> where, 
> - <font  color="DodgerBlue"> ($x_1, \dots, x_d$) are input features </font>
> - <font  color="red"> ($w_1, \dots, w_d$) are coefficients or weights </font> (learned from the data)
> - <font  color="green"> $b$ is the bias which can be used to offset your hyperplane </font> (learned from the data)


## [[Ridge]]
- `scikit-learn` has a model called `LinearRegression` for linear regression. 
- But if we use this "vanilla" version of linear regression, it may result in large coefficients and unexpected results. 
- So instead of using `LinearRegression`, we will _always use another linear model called `Ridge`_, which is a linear regression model with a complexity **hyperparameter** `alpha`.
```python
from sklearn.linear_model import LinearRegression  # DO NOT USE IT IN THIS COURSE
from sklearn.linear_model import Ridge  # USE THIS INSTEAD
```
### Example
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
### Hyperparameters
#### Alpha
- this hyperparameter is what makes `Ridge` different from `LinearRegression`
	- `alpha=0` is the same as using `LinearRegression`
- controls the #FundamentalTradeoff
- larger `alpha` -> likely to underfit
- smaller `alpha` -> likely to overfit

Examining the effect of `alpha` on the #FundamentalTradeoff 
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

#### Coefficients
- **one coefficient per feature** which describes the **role/weight** of the feature in the prediction according to the model
- **positive coefficient** means the prediction is ***proportional*** to the feature value
- **negative coefficient** means the prediction is ***inversely proportional*** to the feature value 
- **bigger magnit