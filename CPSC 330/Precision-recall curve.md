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
## Operating point 
- Setting a **requirement on a classifier** (e.g., recall of >= 0.75) is called setting the **operating point**. 
- It's usually driven by **business goals** and is useful to make performance guarantees to customers. 
### Precision/Recall tradeoff 
- But there is a trade-off between precision and recall. 
- If you identify more things as "fraud", 
    - recall is going to increase but 
    - there are likely to be more false positives.
### Decreasing the threshold
- ***Decreasing the threshold*** means a lower bar for predicting fraud. 
    - You are willing to risk more false positives FP⬆ in exchange of more true positives TP⬆. 
      - In general, predicted positives (TP + FP)⬆ go up or stay the same
      - In general, predicted negatives (TN + FN)⬇ go down or stay the same
    - Recall is likely to go up or stay the same
      - TP⬆ / (TP⬆+FN⬇) so generally recall ⬆
    - Precision is likely to go down or stay the same
      - TP⬆ / (TP⬆+FP⬆) so generally precision ⬇
    - Occasionally, precision may increase if all the new examples after decreasing the threshold are TPs. 
### Increasing the threshold
- Increasing the threshold means a higher bar for predicting fraud. 
    - Recall would go down or stay the same but precision is likely to go up 
    - Occasionally, precision may go down if TP decrease but FP do not decrease.