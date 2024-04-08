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