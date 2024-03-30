# Content-Based Filtering
- [[Supervised Learning]]
- use other info besides just rating data
	- e.g. genre, topic, age, location, etc.
- treat rating predictions as a **set of regression problems**
- given movie information, create a profile for each user
	- i.e. build a regression model for each user, and learn weights
	- based on
		- movies they have watched
		- ratings for the movies
		- features of the movies
## Regression Algorithm
For each user $i$, create a user profile as follows:
- Consider all movies rated by $i$ and create `X` and `y` for the user
    - Each **row in `X`** contains the **movie features** of movie $j$ rated by $i$
    - Each **value in `y`** is the corresponding **rating** given to the movie $j$ by user $i$
- Fit a regression model using `X` and `y`.
- Apply the model to predict ratings for new items
## Building User Profiles
```python
from collections import defaultdict

user_key = "user_id"
item_key = "movie_id"

user_mapper = dict(zip(np.unique(toy_ratings[user_key]), list(range(N))))
item_mapper = dict(zip(np.unique(toy_ratings[item_key]), list(range(M))))
user_inverse_mapper = dict(zip(list(range(N)), np.unique(toy_ratings[user_key])))
item_inverse_mapper = dict(zip(list(range(M)), np.unique(toy_ratings[item_key])))

def get_lr_data_per_user(ratings_df, d):
    lr_y = defaultdict(list)
    lr_X = defaultdict(list)
    lr_items = defaultdict(list)

    for index, val in ratings_df.iterrows():
        n = user_mapper[val[user_key]]
        m = item_mapper[val[item_key]]
        lr_X[n].append(Z[m])
        lr_y[n].append(val["rating"])
        lr_items[n].append(m)

    for n in lr_X:
        lr_X[n] = np.array(lr_X[n])
        lr_y[n] = np.array(lr_y[n])

    return lr_X, lr_y, lr_items
    
d = movie_feats_df.shape[1]
X_train_usr, y_train_usr, rated_items = get_lr_data_per_user(toy_ratings, d)
```