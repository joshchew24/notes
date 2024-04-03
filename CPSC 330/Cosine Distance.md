# Cosine Distance
Cosine similarity: normalized version of [[Dot Product]]. 
$$similarity_{cosine}(vec1,vec2) = \frac{vec1.vec2}{\left\lVert vec1\right\rVert_2 \left\lVert vec2\right\rVert_2}$$

Where, 
- The L2 norm of $vec1$ is the magnitude of $vec1$
      $$\left\lVert vec1\right\rVert_2 = \sqrt{\sum_i vec1_i^2}$$
- The L2 norm of $vec2$ is the magnitude of $vec2$
  $$\left\lVert vec2\right\rVert_2 = \sqrt{\sum_i vec2_i^2}$$

```python
  # Cosine Similarity
cosine_similarity = np.dot(vec1, vec2) / (np.linalg.norm(vec1) * np.linalg.norm(vec2))
print(f"Cosine Similarity: {cosine_similarity:.4f}")
```

