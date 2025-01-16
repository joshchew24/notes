# Class Imbalance
## Class Imbalance
- when the dataset classes have a highly disproportional distribution of examples
- we can obtain very high accuracy scores even if we are frequently mislabelling one class with lower representation
- Real world data is often imbalanced. 
    - Our Credit Card Fraud dataset is imbalanced.
    - Ad clicking data is usually drastically imbalanced. (Only around ~0.01% ads are clicked.)
    - Spam classification datasets are also usually imbalanced.
- can be thought of as **anomaly detection**
## Addressing Imbalance
- why does it exist?
	- is one class much rarer than the other?
		- do we care about one type of error more than the other?
	- is it because of data collection methods?
		- then our test and training data come from different distributions
- sometimes, we can just ignore
## Handling Imbalance
- can we change the model instead of adjusting the threshold?
- common approaches
	- changing the data #optional
		- undersampling
		- oversampling
			- random oversampling
			- SMOTE
	- changing the training procedure
		- `class_weight` 
## Changing the training procedure
- All `sklearn` classifiers have a parameter called `class_weight`.
- This allows you to specify that one class is more important than another.
- For example, maybe a false negative is 10x more problematic than a false positive. 
### Example: `class_weight` parameter of `sklearn LogisticRegression` 
> class sklearn.linear_model.LogisticRegression(penalty='l2', dual=False, tol=0.0001, C=1.0, fit_intercept=True, intercept_scaling=1, **class_weight=None**, random_state=None, solver='lbfgs', max_iter=100, multi_class='auto', verbose=0, warm_start=False, n_jobs=None, l1_ratio=None)

> class_weight: dict or 'balanced', default=None

> Weights associated with classes in the form {class_label: weight}. If not given, all classes are supposed to have weight one. 

```python
ConfusionMatrixDisplay.from_estimator(
    pipe_lr, X_valid, y_valid, display_labels=["Non fraud", "fraud"], values_format="d"
);
```
![[Pasted image 20240225013224.png]]
```python
pipe_lr_weight = make_pipeline(
    StandardScaler(), LogisticRegression(max_iter=500, class_weight={0: 1, 1: 10})
)

pipe_lr_weight.fit(X_train, y_train)
ConfusionMatrixDisplay.from_estimator(
    pipe_lr_weight,
    X_valid,
    y_valid,
    display_labels=["Non fraud", "fraud"],
    values_format="d",
);
```
![[Pasted image 20240225013234.png]]
- Notice we've **reduced false negatives** and predicted more Fraud this time.
- This was equivalent to saying give 10x more "importance" to fraud class.
- Note that as a consequence we are also **increasing false positives**.
#### `class_weight="balanced"`
- A useful setting is `class_weight="balanced"`.
- This sets the weights so that the classes are "equal".

> class_weight: dict, ‘balanced’ or None
If ‘balanced’, class weights will be given by `n_samples / (n_classes * np.bincount(y))`. If a dictionary is given, keys are classes and values are corresponding class weights. If None is given, the class weights will be uniform.

> `sklearn.utils.class_weight.compute_class_weight(class_weight, classes, y)`

```python
pipe_lr_balanced = make_pipeline(
    StandardScaler(), LogisticRegression(max_iter=500, class_weight="balanced")
)
pipe_lr_balanced.fit(X_train, y_train)
ConfusionMatrixDisplay.from_estimator(
    pipe_lr_balanced,
    X_valid,
    y_valid,
    display_labels=["Non fraud", "fraud"],
    values_format="d",
);
```
![[Pasted image 20240225013357.png]]
##### Evaluate Performance Gain
```python
comp_dict = {}
pipe_lr = make_pipeline(StandardScaler(), LogisticRegression(max_iter=500))
scoring = ["accuracy", "f1", "recall", "precision", "roc_auc", "average_precision"]
orig_scores = cross_validate(pipe_lr, X_train_big, y_train_big, scoring=scoring)

pipe_lr_balanced = make_pipeline(
    StandardScaler(), LogisticRegression(max_iter=500, class_weight="balanced")
)
scoring = ["accuracy", "f1", "recall", "precision", "roc_auc", "average_precision"]
bal_scores = cross_validate(pipe_lr_balanced, X_train_big, y_train_big, scoring=scoring)
comp_dict = {
    "Original": pd.DataFrame(orig_scores).mean().tolist(),
    "class_weight='balanced'": pd.DataFrame(bal_scores).mean().tolist(),
}

pd.DataFrame(comp_dict, index=bal_scores.keys())
```
- Recall is much better but precision has dropped a lot; we have many false positives. 
- You could also optimize `class_weight` using hyperparameter optimization for your specific problem. 
- Changing the class weight will **generally reduce accuracy**.
- The original model was trying to maximize accuracy.
- Now you're telling it to do something different.
- But that can be fine, accuracy isn't the only metric that matters.

## See also
[[Data Splitting#Stratified Splits|Stratified Splits]]