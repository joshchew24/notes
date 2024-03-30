---
aliases:
  - DBSCAN
---
# Density-Based Spatial Clustering of Applications with Noise
## Overview
- density-based clustering algorithm
- based on the idea that **clusters form dense regions** in the data
	- **identifies "crowded" regions** in the feature space
- addresses some limitations of [[K-Means Clustering]]
	- does not require number of clusters in advance
	- can identify points that are not part of any clusters
	- can capture clusters of complex shapes
	- ![[Pasted image 20240329182858.png]]
	- ![[Pasted image 20240329182932.png]]
## Hyperparameters
### `eps`
- determines what it means for points to be "close"
	- i.e. determines cluster density
- smaller `eps` gives denser clusters
	- more computationally expensive
- larger `eps` merges neighbouring clusters
	- adds more noise points into clusters
	- fewer clusters
### `min_samples`
- determines the number of **neighbouring points** we require to consider in order for a point to be a part of a cluster