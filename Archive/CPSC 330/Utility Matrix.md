# Utility Matrix
- most often, data comes in as ratings for a set of items from a set of users
- $N$ users and $M$ items
- **users** are consumers
- **items** are the products or services offered
    - e.g. movies (Netflix), books (Amazon), songs (spotify), people (tinder) 
- a **utility matrix** captures **interactions** between $N$ users and $M$ items
	- ratings, click, purchases, etc.
	- **sparsity** due to users usually only interacting with a few items
		- all Netflix users will have rated only a small percentage of content available on Netflix
	    - all amazon clients will have rated only a small fraction of items among all items available on Amazon
	- **recommendation systems** will try to **complete the utility matrix** by **predicting missing values**
![[Pasted image 20240329005929.png]]
## Creating a Utility Matrix Example
### Loading Dataset
- [Jester 1.7M jokes ratings dataset](https://www.kaggle.com/datasets/vikashrajluhaniwal/jester-17m-jokes-ratings-dataset/data)
	- Download `jester_ratings.csv` and `jester_items.csv`
		- ratings (-10.0 to +10.0) of 150 jokes from 59,132 users. 
		- joke IDs and the actual text of jokes. 
```python
filename = "../data/jester_ratings.csv"
ratings_full = pd.read_csv(filename)
# this takes a small sample for speed
ratings = ratings_full[ratings_full["userId"] <= 4000]

ratings.head()

user_key = "userId"
item_key = "jokeId"

ratings.info()

def get_stats(ratings, item_key="jokeId", user_key="userId"):
    print("Number of ratings:", len(ratings))
    print("Average rating:  %0.3f" % (np.mean(ratings["rating"])))
    N = len(np.unique(ratings[user_key]))
    M = len(np.unique(ratings[item_key]))
    print("Number of users (N): %d" % N)
    print("Number of items (M): %d" % M)
    print("Fraction non-nan ratings: %0.3f" % (len(ratings) / (N * M)))
    return N, M


N, M = get_stats(ratings)
```
### Creating the Matrix
```python
# from lecture 16

user_mapper = dict(zip(np.unique(ratings[user_key]), list(range(N))))
item_mapper = dict(zip(np.unique(ratings[item_key]), list(range(M))))
user_inverse_mapper = dict(zip(list(range(N)), np.unique(ratings[user_key])))
item_inverse_mapper = dict(zip(list(range(M)), np.unique(ratings[item_key])))

def create_Y_from_ratings(
    data, N, M, user_mapper, item_mapper, user_key="userId", item_key="jokeId"
):  # Function to create a dense utility matrix
    Y = np.zeros((N, M))
    Y.fill(np.nan)
    for index, val in data.iterrows():
        n = user_mapper[val[user_key]]
        m = item_mapper[val[item_key]]
        Y[n, m] = val["rating"]

    return Y
Y_mat = create_Y_from_ratings(ratings, N, M, user_mapper, item_mapper)
pd.DataFrame(Y_mat)
```
- Rows represent users, columns represent items (jokes in our case), each cell gives the rating given by the user to the corresponding joke. 
- **Users are features for jokes** and **jokes are features for users**.
### Data Splitting
- done for prediction evaluation
	- allows us to hide some data from training for scoring later on
- split into train and validation sets
- easier to **split the ratings data** instead of the utility matrix
- can pretty much ignore `y`
```python
X = ratings.copy()
y = ratings[user_key]
X_train, X_valid, y_train, y_valid = train_test_split(
    X, y, test_size=0.2, random_state=42
)

X_train.shape, X_valid.shape

# create utility matrices for the splits
train_mat = create_Y_from_ratings(X_train, N, M, user_mapper, item_mapper)
valid_mat = create_Y_from_ratings(X_valid, N, M, user_mapper, item_mapper)

train_mat.shape, valid_mat.shape
```
- The training matrix `train_mat` is of shape N by M but only has ratings from `X_train` and all other ratings missing. 
- The validation matrix `valid_mat` is also of shape N by M but it only has ratings `X_valid` and all other ratings missing. 
- They have the same shape because both have the same number of users and items; that's how we have constructed them. 
