# Clustering
> [!Definition]
> The task of partitioning the dataset into groups called clusters based on their similarities
- discover underlying groups such that
	- examples in same group are as similar as possible
	- examples in different groups are as different as possible
## Basics
### Cluster Labels
- used to identify a cluster
- completely arbitrary, label switching (relabelling) does not make a difference
- in real-world data, clusters are rarely visually separable when
### Correct Grouping
- meaningful groups are dependent on the **application**
- helps to have prior knowledge about data and problem
- hard to objectively measure quality of a clustering algorithm
![[Pasted image 20240329161832.png]]
### Common Applications
- data exploration
	- summarize or compress data
	- partition data into groups before further processing
	- e.g. using clustering in a supervised learning setting
		- perform clustering and examine performance of **supervised** model on individual clusters
		- if performance is lower on a particular cluster, try building a separate model for it
- customer segmentation
	- understand landscape of market in businesses
	- craft targeted business or marketing strategies tailored for each group
	- ![[Pasted image 20240329162148.png|175]]
- document clustering
	- grouping articles on different topics from different news sources
- social network analysis
- medical imaging
- imputing missing data, data compression, privacy preservation

## Methods
- [[K-Means Clustering]]
	- Clustering is based on the notion of **similarity or distances** between points. 
	- How do we determine similarity between points in a multi-dimensional space?
	- Can we use something like $k$-neighbours for similarity? 
	    - Yes! That's a good start!  
	    - With $k$-neighbours we used Euclidean distances to find nearby points. 
	    - We can use the same idea for clustering! 
- [[Density-Based Spatial Clustering of Applications with Noise|DBSCAN]]
- [[Hierarchical Clustering]]