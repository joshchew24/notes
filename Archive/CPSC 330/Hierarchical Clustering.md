# Hierarchical Clustering
- motivation:
	- difficult to decide how many clusters
	- helpful to first get a complete picture of similarity between points
- main idea:
	1. start with each point in its own cluster
	2. greedily merge most similar clusters
	3. repeat step 2 until you obtain only one cluster ($n-1$ times)
## [[Dendrogram]]
Used to visualize the clustering
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
### Flatten
flatten a dendrogram to obtain labels based on the max number of clusters
```python
from scipy.cluster.hierarchy import fcluster

# flattening the dendrogram based on maximum number of clusters. 
hier_labels1 = fcluster(linkage_array, 3, criterion="maxclust") 
hier_labels1

plot_dendrogram_clusters(X, linkage_array, hier_labels1, title="flattened with max_clusts=3")
```
![[Pasted image 20240325001004.png]]
can also flatten based on the maximum distance between points
```python
# flattening the dendrogram based on maximum distance between points. 
hier_labels2 = fcluster(linkage_array, 1.5, criterion="distance") 
hier_labels2
plot_dendrogram_clusters(X, linkage_array, hier_labels2, title="flattened with dist=1.5")
```
![[Pasted image 20240325001047.png]]
### Linkage Criteria
- when creating a dendogram, we need to calculate **distance between clusters**
![[Pasted image 20240325004024.png|700]]
#### `single` linkage
- merges two clusters that have the **smallest minimum distance** between all their point
- leads to loose clusters
- use `scipy.cluster.hierarchy`'s `single` to get linkage information
- gives us matrix `Z` with the merging information
```python
Z = single(X)
columns = ["c1", "c2", "distance(c1, c2)", "# observations"]
pd.DataFrame(Z, columns=columns).head()
```
![[Pasted image 20240325001705.png]]
- The linkage returns a matrix `Z` of shape `n-1` (number of iterations) by 4:
- The **rows represent iterations**.
- First and second columns (c1 and c2 above): **indexes of the clusters** being merged.
- Third column (distance(c1, c2)): the **distance between the clusters** being merged.
- Fourth column (# observations): the number of **examples in the newly formed cluster**.
![[Pasted image 20240325001846.png|260]] ![[Pasted image 20240325001850.png|256]]
#### `complete` linkage
- merges two clusters that have the **smallest maximum distance** between their points
![[Pasted image 20240325003520.png|277]]![[Pasted image 20240325003530.png|286]]
#### `average` linkage
- merges two clusters that have the **smallest average distance** between all their points
#### `ward` linkage
- picks two clusters to merge such that the **variance within all clusters increases the least**
- often leads to equally sized clusters
### Truncation
- what if we have **thousands of examples**?
- two strategies:
	- `level` $\rightarrow$ "Maximum depth" of the tree is $p$
		- i.e. only $p$ levels of the dendrogram tree are displayed
			- a “level” includes all nodes within $p$ merges from the final merge.
	- `lastp` $\rightarrow$ Only $p$ leaves are shown 
```python
Z = single(X)
dendrogram(Z, p=2, truncate_mode="level");
# p is related to the max depth of the tree
```
![[Pasted image 20240325004354.png]]
```python
dendrogram(Z, p=5, truncate_mode="lastp");
# p is the number of leaf nodes
```
![[Pasted image 20240325004359.png]]![[Pasted image 20240325004455.png]]
