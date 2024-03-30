---
aliases:
  - DBSCAN
---
# Density-Based Spatial Clustering of Applications with Noise
## Overview
- density-based clustering algorithm
- based on the idea that **clusters form dense regions** in the data
	- **identifies "crowded" regions** in the feature space
- there is no `predict` method
	- only clusters points you have, not "new" or "test" points
- addresses some limitations of [[K-Means Clustering]]
	- does not require number of clusters in advance
	- can identify points that are not part of any clusters
	- can capture clusters of complex shapes
	- ![[Pasted image 20240329182858.png]]
	- ![[Pasted image 20240329182932.png]]
## Hyperparameters
### `eps`
- i.e. epsilon
- determines what it means for points to be "close"
	- i.e. determines cluster density
- smaller `eps` gives denser clusters
	- more computationally expensive
- larger `eps` merges neighbouring clusters
	- adds more noise points into clusters
	- fewer clusters
### `min_samples`
- determines the number of **neighbouring points** we require to consider in order for a point to be a part of a cluster
	- i.e. minimum number of points required to form a dense region or cluster
	- determines granularity of the clusters and their resilience to noise
- smaller `min_samples` give looser clusters that encompass fewer points
	- more prone to labelling points as noise
	- smaller, more fragmented clusters
	- numerous clusters
	- computationally expensive
- larger `min_samples` give denser and more compact clusters
	- less sensitive to noise
		- i.e. noise points are less likely to form their own clusters since they're less likely to meet the density criterion
	- larger clusters
	- fewer clusters
## Points
- Core Points
	- points that have at least `min_samples` points within a distance of `eps`
- border points
	- connected to a core point
	- within a distance of `eps` to a core point
	- have fewer than `min_samples` points within a distance of `eps`
- noise points
	- do not belong to any cluster
	- have fewer than `min_samples` points within distance `eps` of the starting point
## Algorithm
- Pick a point $p$ at random.
- Check if $p$ is a core point
	- look at the number of neighbours within `eps`
	- core if they are near at least `min_samples` points
- If $p$ is a core point, give it a colour (label)
- Spread the colour of $p$ to all of its neighbours
- Check if any of the neighbours that received the colour is a core point
- if yes, spread the colour to its neighbors as well
	- if not, it is a border point
- Once there are no more core points left to spread the colour, pick a new unlabeled point $p$ and repeat the process.