# R squared $R^2$
- common score
- used by default by `sklearn` when calling `score()`
- measures the proportion of variability in $y$ that can be explained using $X$
- indepent of the scale of $y$
	- max is 1
$$R^2(y, \hat{y}) = 1 - \frac{\sum_{i=1}^n (y_i - \hat{y_i})^2}{\sum_{i=1}^n (y_i - \bar{y})^2}$$
- The denominator measures the total variance in �.
- The amount of variability that is left unexplained after performing regression.
- The maximum is 1 for perfect predictions
- Negative values are very bad: "worse than DummyRegressor" (very bad)