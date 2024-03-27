# Feature Cross
- synthetic feature formed by multiplying or crossing two or more features
## Example
### Small (XOR)
Is the following dataset (XOR function) linearly separable? 

| $$x_1$$ | $$x_2$$ | target |     |
| ------- | ------- | ------ | --- |
| 1       | 1       | 0      |     |
| -1      | 1       | 1      |     |
| 1       | -1      | 1      |     |
| -1      | -1      | 0      |     |
```python
import seaborn as sb
X = np.array([
    [-1, -1],
    [1, -1],
    [-1, 1],
    [1, 1]
])
y = np.array([1, 0, 0, 1])
df = pd.DataFrame(np.column_stack([X, y]), columns=["X1", "X2", "target"])
plt.figure(figsize=(4, 4))
sb.scatterplot(data=df, x="X1", y="X2", style="target", s=200, legend=False);
```
![[Pasted image 20240327060617.png|350]]
No line can be drawn to separate the target class. If we create a feature crsoss $x1x2$, the data becomes linearly separable.

| $$x_1$$ | $$x_2$$ | $$x_1x_2$$ | target|
|---------|---------|---------|---------|
| 1 | 1  | 1 | 0|
| -1 | 1  | -1 | 1|
| 1 | -1  | -1 | 1|
| -1 | -1  | 1 | 0|    
```python
df["X1X2"] = df["X1"] * df["X2"]
plt.figure(figsize=(4, 4))
sb.scatterplot(data=df, x="X2", y="X1X2", style="target", s=200, legend=False);
```
![[Pasted image 20240327061234.png]]

### More Complex
```python
xx, yy = np.meshgrid(np.linspace(-3, 3, 50), np.linspace(-3, 3, 50))
rng = np.random.RandomState(0)
rng.randn(4, 2)  # example output

X_xor = rng.randn(200, 2)
y_xor = np.logical_xor(X_xor[:, 0] > 0, X_xor[:, 1] > 0)
# Interaction term
Z = X_xor[:, 0] * X_xor[:, 1]

# Z == x*y or x1x2
df = pd.DataFrame({'X': X_xor[:, 0], 'Y': X_xor[:, 1], 'Z': Z, 'Class': y_xor})
df.head()

plt.scatter(df[df['Class'] == True]['X'], df[df['Class'] == True]['Y'], c='blue', label='Class 0', s=50)
plt.scatter(df[df['Class'] == False]['X'], df[df['Class'] == False]['Y'], c='red', label='Class 0', s=50);
```
![[Pasted image 20240327061329.png]]
```python
LogisticRegression().fit(X_xor, y_xor).score(X_xor, y_xor)

from sklearn.preprocessing import PolynomialFeatures
pipe_xor = make_pipeline(
    PolynomialFeatures(interaction_only=True, include_bias=False), LogisticRegression()
)
pipe_xor.fit(X_xor, y_xor)
pipe_xor.score(X_xor, y_xor)

feature_names = (
    pipe_xor.named_steps["polynomialfeatures"].get_feature_names_out().tolist()
)

pd.DataFrame(
    pipe_xor.named_steps["logisticregression"].coef_.transpose(),
    index=feature_names,
    columns=["Feature coefficient"],
)
```
The interaction feature has the biggest coefficient!

## [[One-hot encoding|One-hot Encoded]] Features
- think of it as logical conjunctions
- 