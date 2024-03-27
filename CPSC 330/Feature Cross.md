# Feature Cross
- synthetic feature formed by multiplying or crossing two or more features
## Example
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