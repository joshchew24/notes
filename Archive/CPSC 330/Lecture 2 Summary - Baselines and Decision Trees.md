# Lecture 2 Summary - Baselines and Decision Trees
## Learning Objectives
- **Identifying problems for [[Supervised Learning|Supervised Machine Learning]]**
	- a problem can be solved with supervised learning if you have labelled data, where each input in the dataset is paired with an output
	- for example, predicing house prices based on their size and location
- **Differentiate between supervised and [[Unsupervised Learning]]**
	- in supervised learning, model learns from labelled data to make predictions
	- in unsupervised learning, the model works with unlabelled data to find patterns/groupings
		- e.g. clustering customers based on purchasing behaviours
- **Explain ML Terminology**
	- **Features**: inputs or variables used to make predictions (e.g. age, income)
	- **Targets**: the output or variable that the model is trying to predict (e.g. creditworthiness)
	- **Predictions**: the output generated by the model
	- **Training**: the process of teaching a model to make predictions by learning from a dataset
	- **Error**: the difference between the model's prediction and the actual values
- **Differentiate between [[Classification]] and [[Regression]] problems**
	- classification involves predicting a category (spam/not spam), regression involves predicting a continuous value (house prices)
- **Using `DummyClassifier` and `DummyRegressor` as [[Baselines]]**
	- these simple models provide a baseline for comparison
	- `DummyClassifier` might predict most frequent class
	- `DummyRegressor` might predict the mean of the target values
	- establish a minimum performance baseline for more sophisticated models
- **explain the `fit` and `predict` paradigm and use `score` method of ML models**
	- **Fit**: train the model on the dataset
	- **Predict**: use the trained model to make predictions on new data
	- **Score**: evaluate the performance of a model 
		- accuracy score for classification models
		- R-squared value for regression models
- **broadly describe how [[Decision Trees]] prediction works**
	- decision trees make predictions by splitting the data based on feature values
	- splits form a tree structure
		- nodes represent decisions based on a feature
		- leaves represent a prediction
- **use `DecisionTreeClassifier` and `DecisionTreeRegressor` to build decision trees using `scikit-learn`;**
	- [[sklearn]] classes used to create decision trees for classification and regression
- **visualize decision trees**
	- `plot_tree` in [[sklearn]] can visualize tree structure
		- shows splits at each node, and decision path for predictions
- **explain the difference between [[Parameters]] and [[Hyperparameters]]**
	- parameters are values learned by the model during training
		- e.g. splits in decision tree
	- hyperparameters are set before training and guide the learning process
		- e.g. max depth of tree
- **explain the concept of [[Decision Boundary]]**
	- in classification, these are the lines/surfaces in the feature space that separate different classes
	- in 2D feature space, decision boundary is a line that separates two classes
- **explain the relation between model complexity and decision boundaries.**
	- more complex models (e.g. deep decision trees) can create intricate decision boundaries
		- can lead to overfitting
		- model captures noise in training data and performs poorly on unseen data