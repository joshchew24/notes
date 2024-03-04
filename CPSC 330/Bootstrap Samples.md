---
aliases:
  - bootstrap sample
---
# Bootstrap Samples
## An example of a bootstrap samples
- Suppose you are training a random forest model with `n_estimators=3`. 
- Suppose this is your original dataset with six examples: [0,1,2,3,4,5]
    - a sample drawn with replacement for tree 1: [1,1,3,3,3,4]
    - a sample drawn with replacement for tree 2: [3,2,2,2,1,1]
    - a sample drawn with replacement for tree 3: [0,0,0,4,4,5]
- Each decision tree trains on a total of six examples.
- Each tree trains on a different set of examples. 