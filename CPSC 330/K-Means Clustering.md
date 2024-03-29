# K-Means Clustering
## Basics
**Input**
- `X` $\rightarrow$ a set of data points  
- `K` (or $k$ or `n_clusters`) $\rightarrow$ number of clusters
**Output**
- `K` clusters (groups) of the data points 
## `sklearn`
```python
from sklearn.cluster import KMeans

X, y = make_blobs(n_samples=10, centers=3, n_features=2, random_state=10)
mglearn.discrete_scatter(X[:, 0], X[:, 1]);

pd.DataFrame(data=X, columns=["feat1", "feat2"])
```
### `fit`
- only pass `X` because we have no labels to fit the model on (this is unsupervised)
```python
kmeans = KMeans(n_clusters=3, n_init='auto')
kmeans.fit(X); # We are only passing X because this is unsupervised learning
```
### `predict`
- output of `KMeans` is `n_clusters` clusters of the data points
- Calling `predict` on the `KMeans` object gives the cluster assignment for each data point
```python
clust_labels = kmeans.predict(X)
clust_labels
```