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
## Utility Matrix
- most often, data comes in as ratings for a set of items from a set of users
- $N$ users and $M$ items
- **users** are consumers
- **items** are the products or services offered
    - e.g. movies (Netflix), books (Amazon), songs (spotify), people (tinder) 
- a **utility matrix** captures **interactions** between $N$ users and $M$ items
	- ratings, click, purchases, etc.
![[Pasted image 20240329005929.png]]
