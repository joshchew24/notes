# Target Transformation
- used in [[Linear Regression]]
- when you have prices or count data, target values are often skewed
- can apply a log transform on the target column to make it more normal
	- i.e. $y\rightarrow log(y)$
- linear regression usually works better on something that looks more normal
## Example
```python
plt.hist(y_train, bins=100)
plt.hist(np.log10(y_train), bins=100)
```

## Pipeline
```python
from sklearn.compose import TransformedTargetRegressor

ttr = TransformedTargetRegressor(
    Ridge(alpha=best_alpha), func=np.log1p, inverse_func=np.expm1
) # transformer for log transforming the target
ttr_pipe = make_pipeline(preprocessor, ttr)

ttr_pipe.fit(X_train, y_train); # y_train automatically transformed
ttr_pipe.predict(X_train)  # predictions automatically un-transformed
mean_absolute_percentage_error(y_test, ttr_pipe.predict(X_test))
```