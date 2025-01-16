- **can be applied to other clustering methods**
- not dependent on cluster centers
- calculated using the **mean intra-cluster distance** ($a$) and the **mean nearest-cluster distance** ($b$) for each sample
>[!Definition] 
>the difference between the **the average nearest-cluster distance** ($b$) and **average intra-cluster distance** ($a$) for each sample, normalized by the maximum value
>$$\frac{b-a}{max(a,b)}$$
- The best value is 1. 
- The worst value is -1 (samples have been assigned to wrong clusters).
- Value near 0 means overlapping clusters. 

- overall **Silhouette score** is the average of the Silhouette scores for all samples
- can visualize the silhouette score for each example individually in a silhouette plot
## Mean intra-cluster distance ($a$)
- select a sample within a cluster
- calculate the average of distances to all other points in the same cluster
## Mean nearest-cluster distance ($b$)
- calculate the average of distances to all other points in each clusters
- select the cluster that is closest and use its mean distance
## Silhouette Plot
- the plots below show the Silhouette scores for each sample in that cluster
- higher values indicate well-separated clusters.
- the size of the Silhouette shows the number of samples and hence shows imbalance of data points in clusters
```python
from yellowbrick.cluster import SilhouetteVisualizer

model = KMeans(2, n_init='auto', random_state=42)
visualizer = SilhouetteVisualizer(model, colors="yellowbrick")
visualizer.fit(XX)  # Fit the data to the visualizer
visualizer.show();
# Silhouette Method
```
![[Pasted image 20240329175701.png|375]]
```python
model = KMeans(5, n_init='auto', random_state=42)
visualizer = SilhouetteVisualizer(model, colors="yellowbrick")
visualizer.fit(XX)  # Fit the data to the visualizer
visualizer.show();
# Finalize and render the figure
```
![[Pasted image 20240329175802.png|375]]
```python
model = KMeans(3, n_init='auto', random_state=42)
visualizer = SilhouetteVisualizer(model, colors="yellowbrick")
visualizer.fit(XX)  # Fit the data to the visualizer
visualizer.show();
# Finalize and render the figure
```
![[Pasted image 20240329175825.png|375]]
### Plot Evaluation
- unlike inertia, larger values are better because they indicate that the point is further away from neighbouring clusters
- the thickness of each silhouette indicates the cluster size
- the shape of the silhouette indicates the "goodness" of each cluster
- a slower dropoff indicates more points are "happy" in their cluster