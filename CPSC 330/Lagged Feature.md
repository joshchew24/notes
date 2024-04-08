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