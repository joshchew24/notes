# Evaluation Metrics
- by default, `.score` returns accuracy
	- accuracy is misleading when there is a class imbalance
- we use [[Precision]], [[Recall]], and [[F1 Score]]
![[Pasted image 20240224225752.png|475]]

Calculate these metrics manually:
```python
data = {
    "calculation": [],
    "accuracy": [],
    "error": [],
    "precision": [],
    "recall": [],
    "f1 score": [],
}
data["calculation"].append("manual")
data["accuracy"].append((TP + TN) / (TN + FP + FN + TP))
data["error"].append((FP + FN) / (TN + FP + FN + TP))
data["precision"].append(precision)  # TP / (TP + FP)
data["recall"].append(recall)  # TP / (TP + FN)
data["f1 score"].append(f1_score)  # (2 * precision * recall) / (precision + recall)
df = pd.DataFrame(data)
df
```

Or use `scikit-learn` functions:
```python
from sklearn.metrics import accuracy_score, f1_score, precision_score, recall_score

data["accuracy"].append(accuracy_score(y_valid, pipe_lr.predict(X_valid)))
data["error"].append(1 - accuracy_score(y_valid, pipe_lr.predict(X_valid)))
data["precision"].append(
    precision_score(y_valid, pipe_lr.predict(X_valid), zero_division=1)
)
data["recall"].append(recall_score(y_valid, pipe_lr.predict(X_valid)))
data["f1 score"].append(f1_score(y_valid, pipe_lr.predict(X_valid)))
data["calculation"].append("sklearn")
df = pd.DataFrame(data)
df.set_index(["calculation"])
```

## Classification Report
```python
from sklearn.metrics import classification_report

print(
    classification_report(
        y_valid, pipe_lr.predict(X_valid), target_names=["non-fraud", "fraud"], digits=4
    )
)
```
```
              precision    recall  f1-score   support

   non-fraud     0.9994    0.9999    0.9996     59708
       fraud     0.8889    0.6275    0.7356       102

    accuracy                         0.9992     59810
   macro avg     0.9441    0.8137    0.8676     59810
weighted avg     0.9992    0.9992    0.9992     59810
```