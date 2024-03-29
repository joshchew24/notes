# K-Means Clustering
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
clust_labels # labels are also accessible through kmeans.labels_

# see the labels next to their inputs
toy_clust_df = pd.DataFrame(X, columns = ['feat1', 'feat2'])
toy_clust_df['cluster labels'] = clust_labels
toy_clust_df
```
## Cluster Centers
- in K-means, each cluster is represented by its **cluster center**
- also known as **centroids**
- average of observations in a cluster
```python
cluster_centers = kmeans.cluster_centers_
cluster_centers

km_labels = kmeans.labels_
mglearn.discrete_scatter(X[:, 0], X[:, 1], kmeans.labels_, markers="o");
plt.legend();
mglearn.discrete_scatter(cluster_centers[:, 0], cluster_centers[:, 1], y =[0,1,2], s=15, markers='*');
```
![[Pasted image 20240329163913.png]]

## Algorithm
- represent each cluster by its cluster center and assign a cluster membership to each datapoint