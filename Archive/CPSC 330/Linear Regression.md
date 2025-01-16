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
