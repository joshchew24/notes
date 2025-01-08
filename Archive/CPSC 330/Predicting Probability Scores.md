# Predicting Probability Scores
- useful to know how confident a model is with a given prediction
	- i.e. "soft" predictions vs. "hard" predictions
- For most of the `scikit-learn` classification models we can access this confidence score or probability score using a method called `predict_proba`.  
## `predict_proba`
- The output of `predict_proba` is the probability of each class. 
- In binary classification, we get probabilities associated with both classes (even though this information is redundant). 
- The first entry is the estimated probability of the first class and the second entry is the estimated probability of the second class from `model.classes_`. 
```python
lr = LogisticRegression(random_state=123)
lr.fit(X_train, y_train)
lr.predict([example])  # hard prediction
lr.predict_proba([example])  # soft prediction
```