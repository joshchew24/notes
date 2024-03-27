# Mean Absolute Percentage Error (MAPE)
- what percentage of the values is our error?
	- e.g. error of $30,000
		- for a house worth $600k, we have 5% error (good)
		- for a house worth $60k, we have 50% error (bad)
## Example
```python
pred_train = ridge_tuned.predict(X_train)
percent_errors = (pred_train - y_train) / y_train * 100.0
np.abs(percent_errors)

# or use a function
def my_mape(true, pred):
    return np.mean(np.abs((pred - true) / true))
```

Or use `sklearn`

```python
from sklearn.metrics import mean_absolute_percentage_error

mean_absolute_percentage_error(y_train, pred_train)
```