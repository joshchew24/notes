# k-Nearest Neighbours
- given a new data point, predict the class of the data point by finding the "closest" data point in the training set
	- i.e. find its "nearest neighbour" or majority vote of nearest neighbours
	- i.e. classify a point based on it's $k$ nearest neighbours labels
## Dimensionality
- to do this, we view data as points in a high dimensional space
	- each relevant feature $d$ of $X$ is represented by one dimensions
- $d \approx 20$ is considered low dimensional
- $d \approx 1000$ is considered medium dimensional 
- $d \approx 100,000$ is considered high dimensional 
## Feature Vectors
- composed of feature values associated with an example
	- correspond to ***rows*** of a dataframe
	- i.e. each example can be represented by a vector in the k-dimensional space
- convert row to numpy vector
	- e.g. `X_spotify.iloc[0].to_numpy()`
### Euclidean Distance
- A common way to calculate the distance between vectors is calculating the **Euclidean distance**. 
- The Euclidean distance 
  - in one dimension between $u$ and $v$: $$distance(u, v) = \sqrt{(u - v)^2} = | u - v |$$

  - in two dimensions between $u = <u_1, u_2>$ and $v = <v_1, v_2>$: $$distance(u, v) = \sqrt{(u_1 - v_1)^2 + (u_2 - v_2)^2}$$

  - generally, in higher dimensions between vectors $u = <u_1, u_2, \dots, u_n>$ and $v = <v_1, v_2, \dots, v_n>$ is defined as: $$distance(u, v) = \sqrt{\sum_{i =1}^{n} (u_i - v_i)^2}$$
  #### Examples
  `two_cities`:
  ![[Pasted image 20240204021823.png]]
- distance between two points
```python
from sklearn.metrics.pairwise import euclidean_distances

euclidean_distances(two_cities)
```
- all distances between all possible point pairs
```python
dists = euclidean_distances(X_cities)
np.fill_diagonal(dists, np.inf)
```
- nearest neighbour 
```python
print("Feature vector for city 0: \n%s\n" % (X_cities.iloc[0]))
print("Distances from city 0 to the first 5 cities: \n%s\n" % (dists[0][:5]))
# We can find the closest city with `np.argmin`:
print(
    "The closest city from city 0 is: %d \n\nwith feature vector: \n%s\n"
    % (np.argmin(dists[0]), X_cities.iloc[np.argmin(dists[0])])
)
```
- new "test" or "query" city
```python
query_point = [[-80, 25]]

dists = euclidean_distances(X_cities, query_point)
print(
    "The query point %s is closest to the city with index %d \n"
    "and the distance between them is: %0.4f"
    % (query_point, np.argmin(dists), dists[np.argmin(dists)])
)
# See the closest city (72) among some other cities with thir distances to query point
X_cities.join(pd.DataFrame(dists, columns=['dist'])).head(np.argmin(dists) + 3).tail()
```

## Choosing `n_neighbors`
- primary [[Hyperparameters|hyperparameter]] is $k$, the number of neighbours voting during prediction
- odd numbers don't give ties
- low $k$ can overfit, high $k$ can underfit
	- if $k = max$ , it's just a dummy classifier
### Hyperparameter Optimization
- cross-validate a bunch of models with different $k$
- examine graph and select sweet spot
#### Example
```python
results_dict = {
    "n_neighbors": [],
    "mean_train_score": [],
    "mean_cv_score": [],
    "std_cv_score": [],
    "std_train_score": [],
}
param_grid = {"n_neighbors": np.arange(1, 50, 5)}

for k in param_grid["n_neighbors"]:
    knn = KNeighborsClassifier(n_neighbors=k)
    scores = cross_validate(knn, X_train, y_train, return_train_score=True)
    results_dict["n_neighbors"].append(k)

    results_dict["mean_cv_score"].append(np.mean(scores["test_score"]))
    results_dict["mean_train_score"].append(np.mean(scores["train_score"]))
    results_dict["std_cv_score"].append(scores["test_score"].std())
    results_dict["std_train_score"].append(scores["train_score"].std())

results_df = pd.DataFrame(results_dict)
results_df = results_df.set_index("n_neighbors")
results_df
```
## Example
```python
X = cities_df.drop(columns=["country"])
y = cities_df["country"]

# split into train and test sets
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.1, random_state=123
)

def f(n_neighbors=1):
    results = {}
    knn = KNeighborsClassifier(n_neighbors=n_neighbors)
    scores = cross_validate(knn, X_train, y_train, return_train_score=True)
    results["n_neighbours"] = [n_neighbors]
    results["mean_train_score"] = [round(scores["train_score"].mean(), 3)]
    results["mean_valid_score"] = [round(scores["test_score"].mean(), 3)]
    print(pd.DataFrame(results))


f(11)
```
## Weights
- when predicting a label, can assign higher weight to examples that are closer to the query example
## Regression
- take the average of the k-nearest neighbours
- can use weighted regression
![[Pasted image 20240205012928.png]]
## Pros
- easy to understand, interpret
- simple hyperparameter $k$ (`n_neighbors`) controlling the fundamental tradeoff
- can learn very complex functions given enough data
- lazy learning: takes no time to `fit`
## Cons
- can be potentially **VERY slow during prediction time**, especially when the training set is very large
- often not great test accuracy compared to modern approaches
- does not work well on datasets with many features
	- or where most feature values rae 0 most of the time (sparse)