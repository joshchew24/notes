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
## Example
```python
ratio = np.bincount(y_train_num)[0] / np.bincount(y_train_num)[1]

from catboost import CatBoostClassifier
from lightgbm.sklearn import LGBMClassifier
from sklearn.tree import DecisionTreeClassifier
from xgboost import XGBClassifier
from sklearn.ensemble import GradientBoostingClassifier, HistGradientBoostingClassifier

pipe_lr = make_pipeline(
    preprocessor, LogisticRegression(max_iter=2000, random_state=123)
)
pipe_dt = make_pipeline(preprocessor, DecisionTreeClassifier(random_state=123))
pipe_rf = make_pipeline(
    preprocessor, RandomForestClassifier(class_weight="balanced", random_state=123)
)
pipe_xgb = make_pipeline(
    preprocessor,
    XGBClassifier(
        random_state=123, verbosity=0
    ),
)
pipe_lgbm = make_pipeline(
    preprocessor, LGBMClassifier(random_state=123, verbose=-1)
)

pipe_catboost = make_pipeline(
    preprocessor,
    CatBoostClassifier(verbose=0, random_state=123),
)

pipe_sklearn_histGB = make_pipeline(
    preprocessor,
    HistGradientBoostingClassifier(random_state=123),
)

pipe_sklearn_GB = make_pipeline(
    preprocessor,
    GradientBoostingClassifier(random_state=123),
)

classifiers = {
    "logistic regression": pipe_lr,
    "decision tree": pipe_dt,
    "random forest": pipe_rf,
    "XGBoost": pipe_xgb,
    "LightGBM": pipe_lgbm,
    "CatBoost": pipe_catboost,
    "sklearn_histGB": pipe_sklearn_histGB,
    "sklearn_GB": pipe_sklearn_GB,
}

results = {}

dummy = DummyClassifier()
results["Dummy"] = mean_std_cross_val_scores(
    dummy, X_train, y_train, return_train_score=True, scoring=scoring_metric
)

for (name, model) in classifiers.items():
    results[name] = mean_std_cross_val_scores(
        model, X_train, y_train_num, return_train_score=True, scoring=scoring_metric
    )

pd.DataFrame(results).T

# comparison of results (excluding the the 'Dummy' row [1:])
cv_score_order = pd.DataFrame(results).T[1:].sort_values('test_score').index
print('\nCV scores:')
print(*cv_score_order, sep=' < ')

fit_time_order = pd.DataFrame(results).T[1:].sort_values('fit_time').index
print('\nFitting speeds:')
print(*fit_time_order, sep=' > ')
```
- Decision trees and random forests overfit
    - Other models do not seem to overfit much. 
- Fit times
    - Decision trees are fast but not very accurate
    - LightGBM is faster than decision trees and more accurate! 
    - CatBoost fit time is highest. 
    - There is not much difference between the validation scores of XGBoost, LightGBM, and CatBoost.
    - Among the best performing models, LightGBM is the fastest one!
- Scores times  
    - Prediction times are much smaller in all cases. 