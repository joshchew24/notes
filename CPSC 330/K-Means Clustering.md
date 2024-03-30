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
>
> **Input**: Data points X and the number of clusters K
> 
> **Initialization**: K initial centers for the clusters
> 
> **Iterative process**:
> 
> repeat 
> - Assign each example to the closest center.
> - Estimate new centers as _average_ of observations in a cluster.
> 
> until **centers stop changing** or **maximum iterations have reached**.

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

**Iterative Process**
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
**Stop!**
- K-means **always converges**
	- doesn't necessarily find the "right" clusters, can converge on a sub-optimal solution
- may not converge before `n_iterations`
## Stochastic Initialization
- randomness can affect the results
- e.g. ![[Pasted image 20240329170453.png]]
- use `n_init` parameter to run the algorithm several times
- use K-means++
	- picks **initial centroids which are far away from each other**
	- `sklearn` does this by default
## Hyperparameter Tuning (`k`)
- need to **decide the number of clusters in advance**
- can't do cross-validation since we don't have target values
### Elbow Method
- examines sum of **intra-cluster distances**, AKA **inertia**
- The intra-cluster distance in the example above is given as   
$$\sum_{P_i \in C_1}  distance(P_i, C_1)^2 + \sum_{P_i \in C_2}  distance(P_i, C_2)^2 + \sum_{P_i \in C_3} distance(P_i, C_3)^2$$
Where 
- $C_1, C_2, C_3$ are centroids 
- $P_i$s are points within that cluster
- $distance$ is the usual Euclidean distance. 
#### Inertia
```python
d = {"K": [], "inertia": []}
for k in range(1, 100, 10):
    model = KMeans(n_clusters=k, n_init='auto').fit(XX)
    d["K"].append(k)
    d["inertia"].append(model.inertia_)
pd.DataFrame(d)
```
- inertia always decreases as K increases
	- can't just look for K that minimizes inertia
		- number of clusters = number of examples has inertia = 0
- must evluate the trade-off: "small k" vs "small inertia"
```python
def plot_elbow(w, h, inertia_values):
    plt.figure(figsize=(w, h))
    plt.axvline(x=3, linestyle="-.", c="black")
    plt.plot(range(1, 10), inertia_values, "-o")
    ax = plt.gca()
    ax.tick_params("both", labelsize=(w + h))
    ax.set_xlabel("K", fontsize=w+h)
    ax.set_ylabel("Inertia", fontsize=w+h)

inertia_values = list()
for k in range(1, 10):
    inertia_values.append(KMeans(n_clusters=k, n_init='auto').fit(XX).inertia_)
plot_elbow(6, 4, inertia_values)
```
![[Pasted image 20240329172259.png]]
- the point of inflection on the curve is our point of diminishing returns on increasing K
##### Yellowbrick
Install using: `conda install -n cpsc330 -c districtdatalabs yellowbrick`
```python
from yellowbrick.cluster import KElbowVisualizer

model = KMeans(n_init='auto')
visualizer = KElbowVisualizer(model, k=(1, 10))

visualizer.fit(XX)  # Fit the data to the visualizer
visualizer.show();

# this makes the optimal number of clusters available in n_clusters
visualizer.finalize()
k_optimal = model.n_clusters
k_optimal
```
### [[Silhouette Method]]

## Limitations
- relies on random initialization
- requires specifying number of clusters in advance
	- rarely know this in advance, and elbow method and silhouette method can be difficult to interpret
- each point has to have an assignment
- clusters are defined solely by its center
	- can only capture relatively simple shapes
	- boundaries between clusters are linear
### Failure Case 1
- performs poorly if clusters have more complex shapes
![[Pasted image 20240329182110.png]]
### Failure Case 2