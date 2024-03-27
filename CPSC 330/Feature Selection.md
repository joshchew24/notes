# Feature Selection
- Find the features (columns) $X$ that are important for predicting $y$, and remove the features that aren't

- Given $X = \begin{bmatrix}x_1 & x_2 & \dots & x_n\\  \\  \\  \end{bmatrix}$ and $y = \begin{bmatrix}\\  \\  \\  \end{bmatrix}$, find the columns $1 \leq j \leq n$ in $X$ that are important for predicting $y$.
- **Interpretability**: Models are more interpretable with fewer features. If you get the same performance with 10 features instead of 500 features, why not use the model with smaller number of features?     
- **Computation**: Models fit/predict faster with fewer columns.
- **Data collection**: What type of new data should I collect? It may be cheaper to collect fewer columns.
- **Fundamental tradeoff**: Can I reduce overfitting by removing useless features?
## Methods
- use **domain knowledge** to **discard** features
- **automatic feature selection**
	- [[Model-Based Selection]]
	- recursive feature elimination
	- forward selection
- looking at [[Feature Importance]]