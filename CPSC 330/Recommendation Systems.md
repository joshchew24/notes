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
## Simple Example
Using the setup from [[Utility Matrix#Creating a Utility Matrix Example|this example]]
### Evaluation
- calculate the **error between actual ratings and predicted ratings**
	- commonly [[Mean Squared Error (MSE)]] or [[Root Mean Squared Error (RMSE)]]
```python
# RMSE
def error(X1, X2):
    """
    Returns the root mean squared error.
    """
    return np.sqrt(np.nanmean((X1 - X2) ** 2))

def evaluate(pred_X, train_X, valid_X, model_name="Global average"):
    print("%s train RMSE: %0.2f" % (model_name, error(pred_X, train_X)))
    print("%s valid RMSE: %0.2f" % (model_name, error(pred_X, valid_X)))
```
### Baselines
#### Global Average
- predict everything as the global average rating
```python
avg = np.nanmean(train_mat)
pred_g = np.zeros(train_mat.shape) + avg
pd.DataFrame(pred_g).head()

evaluate(pred_g, train_mat, valid_mat, model_name="Global average")
```
#### [k-nearest neighbours imputation](https://scikit-learn.org/stable/modules/generated/sklearn.impute.KNNImputer.html)
- Impute missing values using the **mean value from $k$ nearest neighbours** found in the training set. 
- Calculate **distances** between **examples** (users) using **features** (jokes) **where neither value is missing**
```python
from sklearn.impute import KNNImputer

imputer = KNNImputer(n_neighbors=10)
train_mat_imp = imputer.fit_transform(train_mat)

pd.DataFrame(train_mat_imp)
```

## Difference to [[Classification]] and [[Regression]]
### Classification or regression
- We have $X$ and targets for some rows in $X$. 
- We want to predict the last column (target column).  
$$
\begin{bmatrix} 
\checkmark & \checkmark & \checkmark  & \checkmark & \checkmark\\
\checkmark & \checkmark & \checkmark  & \checkmark & \checkmark\\
\checkmark & \checkmark & \checkmark  & \checkmark & \checkmark\\
\checkmark & \checkmark & \checkmark  & \checkmark & ?\\
\checkmark & \checkmark & \checkmark  & \checkmark & ?\\
\checkmark & \checkmark & \checkmark  & \checkmark & ?\\
\end{bmatrix}
$$
### Rating prediction 
- Ratings data has many missing values in the utility matrix. There is **no special target column**. We want to predict the missing entries in the matrix. 
- Since our goal is to **predict ratings**, usually the utility matrix is referred to as $Y$ matrix. 
$$
\begin{bmatrix} 
? & ? & \checkmark  & ? & \checkmark\\
\checkmark & ? & ?  & ? & ?\\
? & \checkmark & \checkmark  & ? & \checkmark\\
? & ? & ?  & ? & ?\\
? & ? & ? & \checkmark & ?\\
? & \checkmark & \checkmark  & ? & \checkmark
\end{bmatrix}
$$

