# Time-Series
- collection of data points indexed in time order
	- weather
	- stocks, market trends
	- energy consumption
	- social sciences
	- sports analytics
## Data Splitting
- regular `train_test_split` randomly samples
	- this breaks the sequential order of our data
	- similar to #GoldenRule, in that we want to train on **past data**, in order to **predict events in the future**
## Example
### Setup
In New York city there is a network of bike rental stations with a subscription system. The stations are all around the city. The anonymized data is available [here](https://ride.citibikenyc.com/system-data).

We will focus on the task is **predicting how many people will rent a bicycle from a particular station for a given time and day**. We might be interested in knowing this so that we know whether there will be any bikes left at the station for a particular day and time.

- The only feature we have is the date time feature. 
    - Example: 2015-08-01 00:00:00
- What's the duration of the intervals? 
    - The data is collected at regular intervals (every three hours) 
- The target is the number of rentals in the next 3 hours. 
    - Example: 3 rentals between 2015-08-01 00:00:00 and 2015-08-01 03:00:00  
```python
import mglearn

citibike = mglearn.datasets.load_citibike()
citibike.head()
```
Visualize:
```python
plt.figure(figsize=(8, 3))
xticks = pd.date_range(start=citibike.index.min(), end=citibike.index.max(), freq="D")
plt.xticks(xticks, xticks.strftime("%a %m-%d"), rotation=90, ha="left")
plt.plot(citibike, linewidth=1)
plt.xlabel("Date")
plt.ylabel("Rentals");
plt.title("Number of bike rentals over time for a selected bike station");
```
### [[#Data Splitting|Split the Data]]:
```python
n_train = 184
train_df = citibike[:184]
test_df = citibike[184:]
plt.figure(figsize=(10, 3))
train_df_sort = train_df.sort_index()
test_df_sort = test_df.sort_index()

plt.plot(train_df_sort, "b", label="train")
plt.plot(test_df_sort, "r", label="test")
plt.xticks(rotation="vertical")
plt.legend()
```
### Try Encoding the Time Feature as [[POSIX Time]]:
In pandas, datetime objects are typically represented as Timestamp objects, which internally store time as the number of nanoseconds (1 billionth of a second. 1 second = $10^9$ nanoseconds) since the POSIX epoch (January 1, 1970, 00:00:00 UTC).
```python
X = (
    citibike.index.astype("int64").values.reshape(-1, 1) // 10 ** 9
)  # convert to POSIX time by dividing by 10**9
y = citibike.values

y_train = train_df.values
y_test = test_df.values
# convert to POSIX time by dividing by 10**9
X_train = train_df.index.astype("int64").values.reshape(-1, 1) // 10 ** 9
X_test = test_df.index.astype("int64").values.reshape(-1, 1) // 10 ** 9
```
- assuming we used random forest, this won't work, because future dates are out of the range of our training data
	- i.e. tree-based models cannot extrapolate to feature ranges outside of the training data
### Helper Function
- Splits the data 
- Trains the given regressor model on the training data
- Shows train and test scores
- Plots the predictions on the train and test data
```python
# Code credit: Adapted from 
# https://learning.oreilly.com/library/view/introduction-to-machine/9781449369880/

def eval_on_features(features, target, regressor, n_train=184, sales_data=False, 
                     ylabel='Rentals', 
                     feat_names="Default", 
                     impute=True):
    """
    Evaluate a regression model on a given set of features and target.

    This function splits the data into training and test sets, fits the 
    regression model to the training data, and then evaluates and plots 
    the performance of the model on both the training and test datasets.

    Parameters:
    -----------
    features : array-like
        Input features for the model.
    target : array-like
        Target variable for the model.
    regressor : model object
        A regression model instance that follows the scikit-learn API.
    n_train : int, default=184
        The number of samples to be used in the training set.
    sales_data : bool, default=False
        Indicates if the data is sales data, which affects the plot ticks.
    ylabel : str, default='Rentals'
        The label for the y-axis in the plot.
    feat_names : str, default='Default'
        Names of the features used, for display in the plot title.
    impute : bool, default=True
        whether SimpleImputer needs to be applied or not

    Returns:
    --------
    None
        The function does not return any value. It prints the R^2 score
        and generates a plot.
    """

    # Split the features and target data into training and test sets
    X_train, X_test = features[:n_train], features[n_train:]
    y_train, y_test = target[:n_train], target[n_train:]

    if impute:
        simp = SimpleImputer()
        X_train = simp.fit_transform(X_train)
        X_test = simp.transform(X_test)
    
    # Fit the model on the training data
    regressor.fit(X_train, y_train)

    # Print R^2 scores for training and test datasets
    print("Train-set R^2: {:.2f}".format(regressor.score(X_train, y_train)))
    print("Test-set R^2: {:.2f}".format(regressor.score(X_test, y_test)))

    # Predict target variable for both training and test datasets
    y_pred_train = regressor.predict(X_train)
    y_pred = regressor.predict(X_test)

    # Plotting
    plt.figure(figsize=(10, 3))

    # If not sales data, adjust x-ticks for dates (assumes datetime format)
    if not sales_data: 
        plt.xticks(range(0, len(X), 8), xticks.strftime("%a %m-%d"), rotation=90, ha="left")

    # Plot training and test data, along with predictions
    plt.plot(range(n_train), y_train, label="train")
    plt.plot(range(n_train, len(y_test) + n_train), y_test, "-", label="test")
    plt.plot(range(n_train), y_pred_train, "--", label="prediction train")
    plt.plot(range(n_train, len(y_test) + n_train), y_pred, "--", label="prediction test")

    # Set plot title, labels, and legend
    title = regressor.__class__.__name__ + "\n Features= " + feat_names
    plt.title(title)
    plt.legend(loc=(1.01, 0))
    plt.xlabel("Date")
    plt.ylabel(ylabel)
```
### Random Forest Regressor:
```python
from sklearn.ensemble import RandomForestRegressor

regressor = RandomForestRegressor(n_estimators=100, random_state=0)
eval_on_features(X, y, regressor, feat_names="POSIX time")
```
