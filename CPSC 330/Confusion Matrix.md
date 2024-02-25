# Confusion Matrix
## Class Imbalance
- when the dataset classes have a highly disproportional distribution of examples
- we can obtain very high accuracy scores even if we are frequently mislabelling one class with lower representation


One way to get a better understanding of the errors is by looking at 
- false positives (type I errors), where the model incorrectly spots examples as fraud
- false negatives (type II errors), where it's missing to spot fraud examples 
```python
from sklearn.metrics import ConfusionMatrixDisplay  # Recommended method in sklearn 1.0

pipe.fit(X_train, y_train)
cm = ConfusionMatrixDisplay.from_estimator(
    pipe, X_valid, y_valid, values_format="d", display_labels=["Non fraud", "fraud"]
)
```
![[Pasted image 20240224224707.png|325]]