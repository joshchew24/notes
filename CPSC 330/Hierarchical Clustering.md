# Hierarchical Clustering
- motivation:
	- difficult to decide how many clusters
	- helpful to first get a complete picture of similarity between points
- main idea:
	1. start with each point in its own cluster
	2. greedily merge most similar clusters
	3. repeat step 2 until you obtain only one cluster ($n-1$ times)
- visualize using a [[Dendogram]]
```python
from scipy.cluster.hierarchy import (
    average,
    complete,
    dendrogram,
    fcluster,
    single,
    ward,
)

X, y = make_blobs(random_state=0, n_samples=11)
linkage_array = ward(X)

plot_X_dendrogram(X, linkage_array, label_n_clusters=True)
```
![[Pasted image 20240325000300.png]]
- flatten a cluster to obtain labels
-