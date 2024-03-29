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
	- craft targeted 