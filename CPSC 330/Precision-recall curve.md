# Precision-recall curve
- Confusion matrix provides a detailed break down of the errors made by the model. 
- But when creating a confusion matrix, we are using "hard" predictions. 
- Most classifiers in `scikit-learn` provide `predict_proba` method (or `decision_function`) which provides **degree of certainty** about predictions by the classifier. 
- Can we explore the degree of uncertainty to understand and improve the model performance? 