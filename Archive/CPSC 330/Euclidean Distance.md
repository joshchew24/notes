# Euclidean Distance

$$distance(vec1, vec2) = \sqrt{\sum_{i =1}^{n} (vec1_i - vec2_i)^2}$$
```python
euclidean_distance = np.linalg.norm(vec1 - vec2)
print(f"Euclidean Distance: {euclidean_distance:.4f}")
```