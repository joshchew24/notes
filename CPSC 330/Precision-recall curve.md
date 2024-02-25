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

Find the values of precision and recall for closest threshold to 0.5:
```python
close_default_index = np.argmin(np.abs(df_PR.index - 0.5))  # closest index to 0.5
df_PR.iloc[[close_default_index]]

# select some table rows (including threshold 0.5) to draw below
rows = np.geomspace(1, len(df_PR), num=10) # pick 10 rows
rows = np.unique(rows.astype(int))  # truncate to integers and drop duplicates
rows = max(rows) - rows  # get reverse geomspace: going from larger to shorter distances
rows = np.append(rows, close_default_index)  # add the index for closest threshold to 0.5
rows = np.sort(rows)
rows = rows[3:-2] # the end points on graph will be too close, so drop some points at both ends
rows # the indices to some thresholds for display in graph below

fig, axes = plt.subplots(1, 2, figsize=(16, 5))

df_PR.plot(ax=axes[0])
axes[0].set_title("Recall and Precision vs Thresholds")
axes[0].set_xlabel("Thresholds")
axes[0].legend(loc="best")

df_PR.plot(x="precision", y="recall", ax=axes[1], label="PR")
df_PR.iloc[rows].plot(
    x="precision", y="recall", style="or", markersize=10, ax=axes[1], label="Thresholds")
axes[1].set_title("Logistic regression: PR Curve")
axes[1].set_xlabel("Precision")
axes[1].set_ylabel("Recall")

for thres, pr in df_PR.iloc[rows].iterrows():
    axes[1].annotate(round(thres, 3), pr)
```

Easier approach which just shows the threshold 0.5:
```python
from sklearn.metrics import precision_recall_curve

precision, recall, thresholds = precision_recall_curve(
    y_valid, pipe_lr.predict_proba(X_valid)[:, 1]
)
plt.plot(precision, recall, label="logistic regression: PR curve")
plt.xlabel("Precision")
plt.ylabel("Recall")
plt.plot(
    precision_score(y_valid, pipe_lr.predict(X_valid)), # by default uses threshold = 0.5
    recall_score(y_valid, pipe_lr.predict(X_valid)),    # by default uses threshold = 0.5
    "or",
    markersize=10,
    label="threshold 0.5",
)
plt.legend(loc="best");
```