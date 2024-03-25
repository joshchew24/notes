# Dendogram
- tree-like plot
- data points on x-axis
- distances between clusters on y-axis
- start with data points as leaves of the tree
- create new parent node for every two clusters that are joined
- length of each branch shows how far the merged clusters go
	- i.e. longer branch means points in clusters are further apart
```python
from scipy.cluster.hierarchy import dendrogram

ax = plt.gca()
dendrogram(linkage_array, ax=ax)
plt.xlabel("Sample index")
plt.ylabel("Cluster distance");
```
![[Pasted image 20240325000300.png]]
