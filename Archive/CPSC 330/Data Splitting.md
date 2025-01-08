# Data Splitting
- set aside randomly selected portion from training data to use as test data
	- proportions are not fixed, usually based on how much data is available
	- can seed the random shuffling using `random_state` argument
- `fit` (train) a model on the training data
- `score` (assess) the trained model on the testing data
	- get a sense of how well the model can generalize
	- pretend the testing data is representative of the real distribution $D$ of data
## [[sklearn]]
- [`sklearn.model_selection.train_test_split`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
```python
train_df, test_df = train_test_split(spotify_df, test_size=0.2, random_state=321)
```
## Validation Data
- additional "test" split taken from the training data
- score the model against the validation split to tune hyperparameters
- not used to `fit` the model
- [[Cross Validation]]
## Deployment Data
- "unseen" data, don't have access to the target values
## Summary
| Type | `fit` | `score` | `predict` |
| ---- | ---- | ---- | ---- |
| Train | x | x | x |
| Validation |  | x | x |
| Test |  | once | once |
| Deployment |  |  | x |
[[Error|generally:]] $E_{train} < E_{validation} < E_{test} < E_{deployment}$
## Stratified Splits
- A similar idea of "[[Class Imbalance|balancing]]" classes can be applied to data splits.
- We have the same option in `train_test_split` with the `stratify` argument. 
- By default it splits the data so that if we have 10% negative examples in total, then each split will have 10% negative examples.
- If you are carrying out cross validation using `cross_validate`, by default it uses [`StratifiedKFold`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html). From the documentation: 

> This cross-validation object is a variation of KFold that returns stratified folds. The folds are made by preserving the percentage of samples for each class.

- In other words, if we have 10% negative examples in total, then each fold will have 10% negative examples.
### Is stratifying a good idea? 

  - Well, it's no longer a random sample, which is probably theoretically bad, but not that big of a deal.
  - If you have many examples, it shouldn't matter as much.
  - It can be especially useful in multi-class, say if you have one class with very few cases.
  - In general, these are difficult questions.