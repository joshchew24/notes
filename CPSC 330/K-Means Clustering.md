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
>[!Goal] 
> Represent each cluster by its cluster center and assign a cluster membership to each datapoint

**Input**: Data points `X` and the number of clusters `K`
```python
n_examples = X.shape[0]
k = 3
```

**Initialization**: `K` initial centers for the clusters (random)
```python
np.random.seed(seed=3)
centers_idx = np.random.choice(range(0, n_examples), size=k)
centers = X[centers_idx]
```
![[Pasted image 20240329164637.png]]

**Iterative process**:

repeat 
- Assign each example to the closest center.
- Estimate new centers as _average_ of observations in a cluster.

until **centers stop changing** or **maximum iterations have reached**.

1. assign each example to closest center
```python
from sklearn.metrics import euclidean_distances

# Distance from each point to each center
dist = euclidean_distances(X, centers)
dist_df = pd.DataFrame(dist, columns=["to c0", "to c1", "to c2"]).rename_axis("from p", axis=1)
Z = np.argmin(dist, axis=1)
dist_df["closest c to p (Z)"] = Z
dist_df

# or use a function
def update_Z(X, centers):
    """
    returns distances and updated cluster assignments
    """
    dist = euclidean_distances(X, centers)
    return dist, np.argmin(dist, axis=1)
```
2. estimate new centers as average of observations in a cluster
```python
# this tells us all the points with label '0'
X[Z == 0]
# this tells us the mean of these points
np.mean(X[z == 2], axis=0)

def update_centers(X, Z, old_centers, k):
    """
    returns new centers
    """
    new_centers = old_centers.copy()
    for kk in range(k):
        new_centers[kk] = np.mean(X[Z == kk], axis=0)
    return new_centers
```