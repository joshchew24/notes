# Data Splitting
- set aside randomly selected portion from training data to use as test data
	- proportions are not fixed, usually based on how much data is available
	- can seed the random shuffling using `random_state` argument
- `fit` (train) a model on the training data
- `score` (assess) the trained model on the testing data
	- get a sense of how well the model can generalize
	- pretend the testing data is representative of the real distribution $D$ of data
## [[sklearn]]
- `sklearn.model_selection.train_test_split`
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


