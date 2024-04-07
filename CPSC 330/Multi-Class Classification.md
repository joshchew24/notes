# Multi-Class Classification
- how do different [[Classification]] models handle multiple classes?
	- linear models don't extend naturally to multiclass
- reduce multiclass into several binary classification problems
## One vs. Rest
- for $k$ classes, train $k$ binary classifiers that tries to separate that class from all other classes
	- 1v{2,3}, 2v{1,3}, 3v{1,2}
- trained on **imbalanced datasets** containing all examples
- given a test point, get scores from all binary classifiers
	- highest score "wins", and is the predicted class of the example
	- each class has a unique set of coefficients and an intercept
## One vs. One
- build a binary model for each pair of classes
	- 1v2, 1v3, 2v3
- trains $\frac{n \times (n-1)}{2}$ binary classifiers
- trained on **relatively balanced subsets**
- given a test point, apply all classifiers
	- count how often each class was predicted
	- predict class with most votes