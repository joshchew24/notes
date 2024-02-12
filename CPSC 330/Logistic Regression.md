# Logistic Regression
- linear **classification** model
- similar to [[Linear Regression]], learns weights associated with each feature and the bias
- applies a **threshold** on the raw output to decide whether the class is positive or negative
	- With logistic regression, the model randomly assigns one of the classes as a positive class and the other as negative. 
    - Usually it would **alphabetically order the target and pick the first one as negative and second one as the positive class.**
- prediction based on the weighted sum of the input features
	- some features either pull prediction towards positive or negative sentiment

> [!Definition]
> A linear classifier is a linear function of the input `X`, followed by a threshold. 
>
> $$\begin{equation}
> \begin{split}
> z =& w_1x_1 + \dots + w_dx_d + b\\
> =& w^Tx + b
> \end{split}
> \end{equation}$$
>
> $$\hat{y} = \begin{cases}
> 	1, & \text{if } z \geq r\\
>    -1, & \text{if } z < r 
>    \end{cases}$$
## Word Example
The model predicts whether a movie review is **positive** or **negative**. It does this by marking the binary presence of certain words (features), and summing the coefficients/weights of the present words. It then classifies the example on whether the sum is positive or negative.
## Components of a linear classifier

1. input features ($x_1, \dots, x_d$)
2. coefficients (weights) ($w_1, \dots, w_d$)
3. bias ($b$ or $w_0$) (can be used to offset your hyperplane)
4. threshold ($r$)

In our example before, we assumed $r=0$ and $b=0$.

## Code Example
```python
from sklearn.linear_model import LogisticRegression

cities_df = pd.read_csv("../data/canada_usa_cities.csv")
train_df, test_df = train_test_split(cities_df, test_size=0.2, random_state=123)
X_train, y_train = train_df.drop(columns=["country"]).values, train_df["country"].values
X_test, y_test = test_df.drop(columns=["country"]).values, test_df["country"].values

cols = train_df.drop(columns=["country"]).columns
train_df.head()

dummy = DummyClassifier()
scores = cross_validate(dummy, X_train, y_train, return_train_score=True)
pd.DataFrame(scores)

lr = LogisticRegression()
scores = cross_validate(lr, X_train, y_train, return_train_score=True)
pd.DataFrame(scores)

# Accessing Learned Parameters
lr = LogisticRegression()
lr.fit(X_train, y_train)
print("Model weights: %s" % (lr.coef_))  # these are the learned weights
print("Model intercept: %s" % (lr.intercept_))  # this is the bias term
data = {"features": cols, "coefficients": lr.coef_[0]}
pd.DataFrame(data)
```
## Calculating Raw Score
```python
y_hat = np.dot(w, x) + b
(
    np.dot(
        example,
        lr.coef_.reshape(
            2,
        ),
    )
    + lr.intercept_
)
```
## Decision Boundary
- hyperplane that divides the feature space in half
```python
lr = LogisticRegression()
lr.fit(X_train, y_train)
discrete_scatter(X_train[:, 0], X_train[:, 1], y_train)
plot_2d_separator(lr, X_train, fill=False, eps=0.5, alpha=0.7)
plt.title(lr.__class__.__name__)
plt.xlabel("longitude")
plt.ylabel("latitude")
```
![[Pasted image 20240212012606.png|300]]
- For $d=2$, the decision boundary is a line (1-dimensional)
- For $d=3$, the decision boundary is a plane (2-dimensional)
- For $d\gt 3$, the decision boundary is a $d-1$-dimensional hyperplane
## Hyperparameters
### `C`
- main hyperparameter that controls the fundamental trade-off
- similar to `C` of [[Support Vector Machines (SVM) with RBF Kernel]]
	- smaller `C` -> might lead to underfitting
	- bigger `C` -> might lead to overfitting
## [[Predicting Probability Scores]]
- The weighted sum $w_1x_1 + \dots + w_dx_d + b$ gives us "raw model output".
- To convert the raw model output into probabilities, instead of taking the sign, we apply the **[[Sigmoid Function]]**.