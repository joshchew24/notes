# Lecture 9 Summary - Classification Metrics
- **Explain why accuracy is not always the best metric in ML**
	- not always a reliable indicator of model performance
		- e.g. if there is a class imbalance
			- could always predict the majority class to achieve high accuracy
			- never correctly predict the minority class (usually the interesting one)
- **Explain components of a [[Confusion Matrix]]**
	- TP, FP, TN, FN

| TN  | FP  |
| --- | --- |
| FN  | TP  |
- **Define precision, recall, and f1-score and use them to evaluate different classifiers**
	- **[[Precision]]**: the percentage of positive predictions that are actually positive
	- **[[Recall]]**: the percentage of actual positives that were predicted as positive
	- **[[F1 Score]]**: harmonic mean of precision and recall
		- single value allows for hyperparameter optimization
		- captures tradeoff between these two measures
- **Broadly explain macro-average, weighted average**
	- **macro-average**: calculates metrics indepentently for each class and then takes the average, treating all classes equally
	- **weighted average**: accounts for class imbalance by weighting the metrics of each class by its *support*
		- the number of true instances for each class
- **Interpret and use [[Precision-recall curve]]
	- plot precision and recall for different thresholds
		- thresholds are the [[Operating Point]] of something like [[Logistic Regression]]
	- useful where class imbalance exists
		- helpful to understand tradeoff between precision/recall for different thresholds
- **Explain average precision score**
	- summarizes the precision-recall curve as teh weighted mean of precisions achieved at each threshold
		- increase in recall from the previous threshold used as the weight
- **Interpret and use [[Receiver Operating Characteristic (ROC) curve|ROC]] curves and ROC AUC using `scikit-learn`**
	- [[Receiver Operating Characteristic (ROC) curve]] 
		- plot the TP rate ([[Recall]]) against FP rate for different thresholds
	- [[Receiver Operating Characteristic (ROC) curve|ROC AUC]]
		- measures the entire 2D area underneath the ROC curve
		- provides an aggregate measure of performance across all possible classification thresholds
- **Identify whether there is [[Class Imbalance]] and whether you need to deal with it**
	- occurs when one class is significantly more frequent than the other
	- important to identify and address
		- can lead to misleadingly high accuracy for the majority class
		- can fail to adequately represent the minorioty class
- **Explain and use `class_weight` to deal with data imbalance**
	- this parameter in classifiers like [[Logistic Regression]] and [[Support Vector Machines (SVM) with RBF Kernel|SVM]]s in [[sklearn]] can be used to give more weight to the minority class
	- balance the influence of each class on the training process
- **Assess model performance on specific groups in a dataset**
	- important to assess a model's performance across different subgroups of the dataset
	- ensure good performance for all groups
		- especially for fairness and bias