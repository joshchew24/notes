# Lagged Feature
- in time series data there is temporal dependence
	- observations close in time tend to be correlated
	- e.g. number of bike rentals at a certain hour is also related to rentals three hours ago
## Example
```python
citibike

rentals_df = pd.DataFrame(citibike)
rentals_df = rentals_df.rename(columns={"one":"n_rentals"})

# helper to create lagged features
def create_lag_df(df, lag, cols):
    return df.assign(
        **{f"{col}-{n}": df[col].shift(n) for n in range(1, lag + 1) for col in cols}
    )

rentals_lag5 = create_lag_df(rentals_df, 5, ['n_rentals'] )

rentals_lag5
```
![[Pasted image 20240407234226.png]]
- first 5 rows have NaN because there is nothing to "shift into"
	- drop or **carefully** impute
- `n_rentals-`$i$ is the $i$th previous example
### Imputing
```python
imp = SimpleImputer()
X_lag_features_imp = imp.fit_transform(X_lag_features)

rentals_lag5_X = pd.DataFrame(X_lag_features_imp, columns=rentals_lag5.drop(columns=['n_rentals']).columns)
rentals_lag5_X.index = rentals_lag5.index
rentals_lag5_X
rentals_lag5_y = rentals_lag5['n_rentals']
```
### Split and Train
```python
# split the given features into a training and a test set
n_train = 184
X_train, X_test = rentals_lag5_X[:n_train], rentals_lag5_X[n_train:]
# also split the target array
y_train, y_test = rentals_lag5_y[:n_train], rentals_lag5_y[n_train:]

rentals_model = RandomForestRegressor(random_state=42)
rentals_model.fit(X_train, y_train);
print("Train-set R^2: {:.2f}".format(rentals_model.score(X_train, y_train)))
print("Test-set R^2: {:.2f}".format(rentals_model.score(X_test, y_test)))
```
## Predicting Future
What do we do if we want to predict a time in the future that has no lag features (i.e. no recent data leading up to the query time)
1. Train a separate model for each number of 3-hour span. E.g. one model that predicts `n_rentals` for next three hours, another model that predicts `n_rentals` in six hours, etc. We can build these datasets.
2. Use a multi-output model that jointly predicts `n_rentalsIn3hours`, `n_rentalsIn6hours`, etc. However, multi-output models are outside the scope of CPSC 330. 
3. Use one model and sequentially predict using a `for` loop. 
	- predict values and pretend they're true

If we decide to use approach 3, we would predict these values and then pretend they are true! For example, given that it's August 31st 2015 at 21:00. 

1. Predict `n_rentals` for September 1st 2015 at 00:00.
2. Then to predict `n_rentals` for September 1st 2015 at 03:00, we need to know `n_rentals` for September 1st 2015 at 00:00. Use our _prediction_ for September 1st 2015 at 00:00 as the truth.
3. Then, to predict `n_rentals` for September 1st 2015 at 06:00, we need to know `n_rentals` for September 1st 2015 at 00:00 and `n_rentals` for September 1st 2015 at 03:00. Use our predictions.
4. Etc etc.