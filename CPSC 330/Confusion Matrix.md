# Confusion Matrix
## [[Class Imbalance]]
- when the dataset classes have a highly disproportional distribution of examples
- we can obtain very high accuracy scores even if we are frequently mislabelling one class with lower representation
## Mispredictions
One way to get a better understanding of the errors is by looking at 
- false positives (type I errors), where the model incorrectly spots examples as fraud
- false negatives (type II errors), where it's missing to spot fraud examples 
```python
# display confusion matrix
from sklearn.metrics import ConfusionMatrixDisplay  # Recommended method in sklearn 1.0

pipe.fit(X_train, y_train)
cm = ConfusionMatrixDisplay.from_estimator(
    pipe, X_valid, y_valid, values_format="d", display_labels=["Non fraud", "fraud"]
)

# get vals into array
from sklearn.metrics import confusion_matrix

predictions = pipe.predict(X_valid)
TN, FP, FN, TP = confusion_matrix(y_valid, predictions).ravel()
TN, FP, FN, TP
# $ (59700, 8, 38, 64)
```
![[Pasted image 20240224224707.png|325]]
- **Perfect** prediction has all values **down the diagonal**
- **Off diagonal** entries can often tell us about what is being **mis-predicted**
### What is "positive" and "negative"?
- Two kinds of binary classification problems 
    - Distinguishing between two classes
    - **Spotting** a class (spot fraud transaction, spot spam, spot disease)
- In case of spotting problems, the thing that we are interested in spotting is considered "**positive**".
- Above we wanted to **spot fraudulent** transactions and so they are "positive". 
## Cross-validation
- can also calculate confusion matrix with cross-validation using the `cross_val_predict` method
- use `ConfusionMatrixDisplay`'s `from_predictions` to draw
```python
from sklearn.model_selection import cross_val_predict

confusion_matrix(y_train, cross_val_predict(pipe, X_train, y_train))
ConfusionMatrixDisplay.from_predictions(
    y_train,
    cross_val_predict(pipe, X_train, y_train),
    display_labels=["Non fraud", "fraud"],
    values_format="d"
);
```

## [[Evaluation Metrics]]
- more metrics to evaluate a model than just `score`
## Which Type of Error is More Important?
- FP and FN have different real-world consequences
	- e.g. diagnosing illness vs convicting criminals
- 