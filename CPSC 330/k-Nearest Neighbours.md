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
#### [[sklearn]]
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