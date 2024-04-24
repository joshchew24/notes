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
			- plot the TP rate ([[Recall]]) against F
- **Identify whether there is class imbalance and whether you need to deal with it**
- **Explain and use `class_weight` to deal with data imbalance**
- **Assess model performance on specific groups in a dataset**