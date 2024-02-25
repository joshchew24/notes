# Precision-recall curve
## [[Operating Point]]
Often, when developing a model, it's not always clear what the operating point will be and to understand the the model better, it's **informative to look at all possible thresholds** and corresponding trade-offs of precision and recall in a plot.  

```python
from sklearn.metrics import precision_recall_curve

precision, recall, thresholds = precision_recall_curve(
    y_valid, pipe_lr.predict_proba(X_valid)[:, 1]
)
print(thresholds.shape, precision.shape, recall.shape)
thresholds = np.append(thresholds, 1)  # make thresholds same length as precision and recall
df_PR = pd.DataFrame(
    {"precision": precision, "recall":recall, "thresholds":thresholds}).set_index("thresholds")
df_PR.sample(10).sort_index()  # see some randomly selected rows
```
- In `precision_recall_curve`, precision and recall are calculated for each threshold.
- The last precision and recall values are 1 and 0 respectively and do not have a corresponding threshold (see 59740 vs. 59741 above).
  - To make shapes compatible, a threshold of 1 is appended to `thresholds`