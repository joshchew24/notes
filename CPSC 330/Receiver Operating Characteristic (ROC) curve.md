# Receiver Operating Characteristic (ROC) curve
- Another commonly used tool to analyze the **behavior of classifiers at different thresholds**.  
- **Similar to PR curve**, it considers all possible thresholds for a given classifier given by `predict_proba` but instead of precision and recall it plots false positive rate (FPR) and true positive rate (TPR or recall).
$$ TPR = \frac{TP}{TP + FN}$$

$$ FPR  = \frac{FP}{FP + TN}$$

- TPR $\rightarrow$ Fraction of true positives out of all positive examples. 
- FPR $\rightarrow$ Fraction of false positives out of all negative examples. 
```python
from sklearn.metrics import roc_curve

fpr, tpr, thresholds = roc_curve(y_valid, pipe_lr.predict_proba(X_valid)[:, 1])
plt.plot(fpr, tpr, label="ROC Curve")
plt.xlabel("FPR")
plt.ylabel("TPR (recall)")

default_threshold = np.argmin(np.abs(thresholds - 0.5))

plt.plot(
    fpr[default_threshold],
    tpr[default_threshold],
    "or",
    markersize=10,
    label="threshold 0.5",
)
plt.legend(loc="best");
```
![[Pasted image 20240225011709.png]]
- Different points on the ROC curve represent different classification thresholds. The curve starts at (0,0) and ends at (1, 1).
    - (0, 0) represents the threshold that classifies everything as the negative class
    - (1, 1) represents the threshold that classifies everything as the positive class 
- The ideal curve is close to the top left
    - Ideally, you want a classifier with high recall while keeping low false positive rate.  
- The red dot corresponds to the threshold of 0.5, which is used by predict.
- We see that compared to the default threshold, we can achieve a better recall of around 0.8 without increasing FPR. 
## Area Under the Curve (AUC)
```python
from sklearn.metrics import roc_auc_score

roc_lr = roc_auc_score(y_valid, pipe_lr.predict_proba(X_valid)[:, 1])
roc_svc = roc_auc_score(y_valid, pipe_svc.decision_function(X_valid))
print("AUC for LR: {:.3f}".format(roc_lr))
print("AUC for SVC: {:.3f}".format(roc_svc))
```
- AUC of **0.5** means **random chance**.
- AUC can be interpreted as evaluating the **ranking of positive examples**.
- What's the probability that a randomly picked positive point has a higher score according to the classifier than a randomly picked point from the negative class. 
- AUC of **1.0** means **all positive points have a higher score than all negative points**.
> [!important]
> For classification problems with imbalanced classes, using AP score or AUC is often much more meaningful than using accuracy. 
## Display
```python
from sklearn.metrics import RocCurveDisplay

RocCurveDisplay.from_estimator(pipe_lr, X_valid, y_valid);
```