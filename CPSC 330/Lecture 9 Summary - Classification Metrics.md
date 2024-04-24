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
- **Interpret and use precision-recall curves**
- **Explain average precision score**
- **Interpret and use ROC curves and ROC AUC using `scikit-learn`**
- **Identify whether there is class imbalance and whether you need to deal with it**
- **Explain and use `class_weight` to deal with data imbalance**
- **Assess model performance on specific groups in a dataset**