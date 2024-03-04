# Gradient Boosted Trees
> [!important] Key Idea
> Combine many simple models (weak learners) to create a **strong learner**
- **No** randomization
- in other words, combine **multiple shallow** (depth 1 to 5) decision trees
- build trees in a **serial manner**, where each tree tries to **correct the mistakes** of the previous one
## Tools
### [XGBoost](https://xgboost.ai/about) 
- Not part of `sklearn` but has similar interface. 
- Install it in your conda environment: <br>`conda install -n cpsc330 -c conda-forge xgboost`
- Supports missing values
- GPU training, networked parallel training
- Supports sparse data
- Typically better scores than random forests
### [LightGBM](https://lightgbm.readthedocs.io/)
- Not part of `sklearn` but has similar interface. 
- Install it in your conda environment:<br> `conda install -n cpsc330 -c conda-forge lightgbm`
- Small model size
- Faster 
- Typically better scores than random forests
### [CatBoost](https://catboost.ai/)
- Not part of `sklearn` but has similar interface. 
- Install it in your course conda environment:<br> `conda install -n cpsc330 -c conda-forge catboost`
- Usually better scores but slower compared to `XGBoost` and `LightGBM`     
## Hyperparameters
- `n_estimators` $\rightarrow$ Number of boosting rounds
- `learning_rate` $\rightarrow$ The learning rate of training
    - controls **how strongly** each tree tries to **correct the mistakes** of the previous trees
    - higher learning rate means each tree can make **stronger corrections**, which means **more complex** model 
- `max_depth` $\rightarrow$ `max_depth` of trees (similar to decision trees) 
- `scale_pos_weight` $\rightarrow$ Balancing of positive and negative weights