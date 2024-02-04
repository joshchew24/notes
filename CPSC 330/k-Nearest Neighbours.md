# k-Nearest Neighbours
- given a new data point, predict the class of the data point by finding the "closest" data point in the training set
	- i.e. find its "nearest neighbour" or majority vote of nearest neighbours
	- i.e. classify a point based on it's $k$ nearest neighbours labels
## Dimensionality
- to do this, we view data as points in a high dimensional space
	- each relevant feature $d$ of $X$ is represented by one dimensions
- $d \approx 20$ is considered low dimensional
- $d \approx 1000$ is considered medium dimensional 
- $d \approx 100,000$ is considered high dimensional 
## Feature Vectors
- composed of feature values associated with an example
	- correspond to ***rows*** of a dataframe
	- i.e. each example can be represented by a vector in the k-dimensional space
### Euclidean Distance
- A common way to calculate the distance between vectors is calculating the **Euclidean distance**. 
- The Euclidean distance 
  - in one dimension between $u$ and $v$: $$distance(u, v) = \sqrt{(u - v)^2} = | u - v |$$

  - in two dimensions between $u = <u_1, u_2>$ and $v = <v_1, v_2>$: $$distance(u, v) = \sqrt{(u_1 - v_1)^2 + (u_2 - v_2)^2}$$

  - generally, in higher dimensions between vectors $u = <u_1, u_2, \dots, u_n>$ and $v = <v_1, v_2, \dots, v_n>$ is defined as: $$distance(u, v) = \sqrt{\sum_{i =1}^{n} (u_i - v_i)^2}$$ 
