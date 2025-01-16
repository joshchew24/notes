# Mean Squared Error (MSE)
- average all the errors $\frac{(predicted - actual)^2}{\#\;predictions}$
- in regression, **target has units**
	- score also depends on this scale
		- e.g. if our target is dollars, and we changed to cents, our MSE would increase by $10,000\times(100^2)$ times
		- `np.mean((y_train * 100 - preds * 100) ** 2)`
- scale the metric to something more relatable using [[Root Mean Squared Error (RMSE)]]
## Example
```python
preds = ridge_tuned.predict(X_train)
np.mean((y_train - preds) ** 2)
```

Or using `sklearn`:

```python
from sklearn.metrics import mean_squared_error

mean_squared_error(y_train, preds)
```