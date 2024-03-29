---
aliases:
  - Recommender Systems
---
# Recommendation Systems
> [!Definition]
> Recommends a particular product or service to users they are likely to consume.

## What Data?
- users ratings data
- features related to items or users
- customer purchase history data
## Approaches
- [[Collaborative Filtering]]
	- type of [[Unsupervised Learning]]
	- only have labels $y_{ij}$ (rating of user $i$ for item $j$)
	- learned features
- [[Content-Based Recommenders]]
	- type of [[Supervised Learning]]
	- extract features $x_i$ of users and/or items
	- build a model to predict rating $y_i$ given $x_i$
	- apply model to predict for new users/items
- Hybrid
	- combining collaborative filtering with content-based filtering
## [[Utility Matrix]]
- **recommendation systems** will try to **complete the utility matrix** by **predicting missing values**