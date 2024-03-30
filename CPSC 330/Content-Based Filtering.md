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
### Building User Profiles
```python
from collections import defaultdict

user_key = "user_id"
item_key = "movie_id"

user_mapper = dict(zip(np.unique(toy_ratings[user_key]), list(range(N))))
item_mapper = dict(zip(np.unique(toy_ratings[item_key]), list(range(M))))
user_inverse_mapper = dict(zip(list(range(N)), np.unique(toy_ratings[user_key])))
item_inverse_mapper = dict(zip(list(range(M)), np.unique(toy_ratings[item_key])))

Z = movie_feats_df.to_numpy()

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
Examine the shapes:
```python
pprint(dict(X_train_usr)) # maps user to array containing arrays of movie features (OHE)
pprint(dict(y_train_usr)) # maps user to array of ratings given to each movie
pprint(dict(rated_items)) # maps user to array of movie IDs they've rated

n = 3
pd.DataFrame(X_train_usr[n], index=rated_items[n]).assign(ratings=y_train_usr[n])
```
Function to examine user profile:
```python
def get_user_profile(user_name):
    X = X_train_usr[user_mapper[user_name]]
    y = y_train_usr[user_mapper[user_name]]
    items = rated_items[user_mapper[user_name]]
    movie_names = [item_inverse_mapper[item] for item in items]
    print("Profile for user: ", user_name)
    profile_df = pd.DataFrame(X, columns=movie_feats_df.columns, index=movie_names)
    profile_df["ratings"] = y
    return profile_df
```
### Training the Model
```python
from sklearn.linear_model import Ridge

def train_for_usr(user_name, model=Ridge()):
    X = X_train_usr[user_mapper[user_name]]
    y = y_train_usr[user_mapper[user_name]]
    model.fit(X, y)
    return model

def predict_for_usr(model, movie_names):
    feat_vecs = movie_feats_df.loc[movie_names].values
    preds = model.predict(feat_vecs)
    return preds
```
Check weights for user "Pat"":
```python
user_name = "Pat"
pat_model = train_for_usr(user_name)
col = "Coefficients for %s" % user_name
pd.DataFrame(pat_model.coef_, index=movie_feats_df.columns, columns=[col])
```
### Predicting
```python
movies_to_pred = ["Roman Holidays", "Malcolm x"]
pred_df = movie_feats_df.loc[movies_to_pred]

user_name = "Pat"
preds = predict_for_usr(pat_model, movies_to_pred)
pred_df[user_name + "'s predicted ratings"] = preds
pred_df