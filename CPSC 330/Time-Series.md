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
[[#Data Splitting|Split Properly]]:
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
Encode the time feature using [[POSIX Time]]:
In pandas, datetime objects are typically represented as Timestamp objects, which internally store time as the number of nanoseconds (1 billionth of a second. 1 second = $10^9$ nanoseconds) since the POSIX epoch (January 1, 1970, 00:00:00 UTC).
```python
X = (
    citibike.index.astype("int64").values.reshape(-1, 1) // 10 ** 9
)  # convert to POSIX time by dividing by 10**9
y = citibike.values
```