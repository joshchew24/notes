# Lecture 3 Summary - Machine Learning Fundamentals
## Learning Objectives
- **explain how decision boundaries change with the `max_depth` hyperparameter**
	- decision boundaries in decision trees are determined by conditions set at each node
	- `max_depth` controls maximum depth of the tree
	- deeper trees have more complex decision boundaries, capturing finer details in data
	- too much depth can cause overfitting
- **explain the concept of generalization**
- **appropriately split a dataset into tran and test sets using `train_test_split` function**
- **explain the difference between train, validation, test, and "deployment" data**
- **identify the difference between training error, validation error, and test error**
- **explain cross-validation and use `cross_val_score` and `cross_validate` to calculate cross-validation error**
- **recognize overfitting and/or underfitting by looking at train and test scores**
- **explain why it is generally not possible to get a perfect test score (zero test error) on a supervised learning problem**
- **describe the fundamental tradeoff between training score and the train-test gap**
- **state the golden rule**
- **start to build a standard recipe for supervised learning: train/test split, hyperparameter tuning with cross-validation, test on test set**