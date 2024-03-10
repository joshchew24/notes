# Root Mean Squared Error (RMSE)
- scales the [[Mean Squared Error (MSE)]] to something more "reasonable"
- how do we know if the error is **acceptable**?
	- [[Mean Absolute Percentage Error (MAPE)]]
## Example
```python
np.sqrt(mean_squared_error(y_train, ridge_tuned.predict(X_train)))
```
