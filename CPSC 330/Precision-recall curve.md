# Precision-recall curve
- Confusion matrix provides a detailed break down of the errors made by the model. 
- But when creating a confusion matrix, we are using "hard" predictions. 
- Most classifiers in `scikit-learn` provide `predict_proba` method (or `decision_function`) which provides **degree of certainty** about predictions by the classifier. 
- Can we explore the degree of uncertainty to understand and improve the model performance? 

Now,
- Suppose for your business it is more costly to miss fraudulent transactions and you want to achieve a **recall of at least 75%** for the "fraud" class. 
- One way to do this is by **changing the threshold** of `predict_proba`.
    - `predict` returns 1 when `predict_proba`'s probabilities are above 0.5 for the "fraud" class.

**Key idea: what if we _threshold the probability at a smaller value_ so that we identify more examples as "fraud" examples?** 
## [[Operating Point]]