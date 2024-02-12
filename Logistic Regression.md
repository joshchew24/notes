# Logistic Regression
- linear **classification** model
- similar to [[Linear Regression]], learns weights associated with each feature and the bias
- applies a **threshold** on the raw output to decide whether the class is positive or negative
- prediction based on the weighted sum of the input features
## Example
The model predicts whether a movie review is **positive** or **negative**. It does this by marking the binary presence of certain words (features), and summing the coefficients/weights of the present words. It then classifies the example on whether the sum is positive or negative.
